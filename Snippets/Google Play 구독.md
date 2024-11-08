---
title: Google Play 구독
date: 2024-11-07
time: 17:05:08
tags:
  - android
  - kotlin
---
## 정기 결제 설정
### Google Play Console에 정기 결제 등록
#### 앱 등록
1. 구글 플레이 콘솔에 앱을 먼저 등록하고 내부 테스트에 app-release.aab를 올린다.
	1. 다만, 내부 테스트에서 올린 앱은 정기 결제를 못 가져 왔기 때문에 일단 임시로 이렇게 적는다. 되는 경우는 기존에 사용하지 않던, 이미 등록된 앱이었음.
	2. 기존에 앱이 업로드 되지 않았다면 업로드 버튼 눌렀을 때 비공개 트랙으로 이동시키기 때문에 비공개 트랙에 올려야 하는지도?
#### 정기 결제 등록
1. 구독 만들기
	1. 여기서 '구독'은 Pro, Premium 혹은 Group 1, Group 2
2. 생성된 구독에서 기본 요금제 만들기
	1. 여기서 '기본 요금제'는 매월, 매년
	2. ID는 1번 구독의 monthly와 2번 구독의 monthly를 다르게 지정하는 편이 쉽다. ex) sub1-monthly, sub2-monthly
3. 기본 요금제 활성화
#### 라이선스 테스트
1. Google Play Console로 이동 (앱 세부 정보 들어가기 이전)
2. 설정 > 라이선스 테스트
3. 테스터 추가

## 구현
### BillingManager.kt
#### 초기화
```kotlin
class BillingManager(private val activity: Activity) {
    private val _subscriptions = MutableStateFlow<List<String>>(emptyList())  
    val subscriptions = _subscriptions.asStateFlow() // 결제한 구독 상품
  
    private val purchasesUpdatedListener = PurchasesUpdatedListener { billingResult, purchases ->  
        if (billingResult.responseCode == BillingResponseCode.OK && purchases != null) {  
            for (purchase in purchases) {  
	            handlePurchase(purchase)  
            }  
        } else {  
            LogHelper.e(billingResult.debugMessage)  
        }  
    } // 상품 구매 업데이트 리스너
  
    private val billingClient = BillingClient.newBuilder(activity).enablePendingPurchases().setListener(purchasesUpdatedListener).build()  
  
    init {  
        // Google Play에 연결  
        billingClient.startConnection(object : BillingClientStateListener {  
            override fun onBillingServiceDisconnected() {  
                LogHelper.e("onBillingServiceDisconnected")  
            }  
            override fun onBillingSetupFinished(p0: BillingResult) {  
                if (p0.responseCode == BillingResponseCode.OK) {  
                    LogHelper.i("onBillingSetupFinished: ${p0.debugMessage}")  
                    hasSubscription()  
                }  
            }
        })
    }
}
```

#### 결제한 구독 상품 가져오기
```kotlin
fun hasSubscription() {  
    val queryPurchaseParams = QueryPurchasesParams.newBuilder().setProductType(ProductType.SUBS).build()  
  
    val purchasesResponseListener = PurchasesResponseListener { billingResult, purchases ->  
        LogHelper.v(billingResult.responseCode)  
        LogHelper.v("$purchases")  
        if (billingResult.responseCode == BillingResponseCode.OK) {  
            for (purchase in purchases) {  
                if (purchase.purchaseState == PurchaseState.PURCHASED) {  
                    _subscriptions.update {  
                        val list = it.toMutableList()  
                        list.addAll(purchase.products)  
                        list  
                    }  
                }
            }
            return@PurchasesResponseListener  
        }  
    }  
  
    billingClient.queryPurchasesAsync(queryPurchaseParams, purchasesResponseListener)  
}
```

#### 구독 상품 구매 가능한지 확인 후 구매
```kotlin
fun checkSubscriptionStatus(subscriptionPlanId: String) {  
    val queryPurchaseParams = QueryPurchasesParams.newBuilder().setProductType(ProductType.SUBS).build()  
  
    val purchasesResponseListener = PurchasesResponseListener { billingResult, purchases ->  
        if (billingResult.responseCode == BillingResponseCode.OK) {  
            for (purchase in purchases) {  
                LogHelper.v("purchase: $purchase")  
                if (purchase.purchaseState == PurchaseState.PURCHASED && purchase.products.contains(subscriptionPlanId)) {  
                    _subscriptions.update {  
                        val list = it.toMutableList()  
                        list.addAll(purchase.products)  
                        list  
                    }  
  
                   return@PurchasesResponseListener  
                }  
            }
        }  
        processPurchases(subscriptionPlanId) // 상품 구매
    }
      
    // 구매가 완료되서 처리  
    billingClient.queryPurchasesAsync(queryPurchaseParams, purchasesResponseListener)  
}
```

#### 구독 상품 구매
```kotlin
private fun processPurchases(subscriptionPlanId: String) {  
	val sub1 = QueryProductDetailsParams.Product.newBuilder().setProductId("sub1").setProductType(ProductType.SUBS).build()  
	val sub2 = QueryProductDetailsParams.Product.newBuilder().setProductId("sub2").setProductType(ProductType.SUBS).build()  

	// 구매 가능한 제품 표시  
	val queryProductDetailsParams = QueryProductDetailsParams.newBuilder().setProductList(listOf(sub1, sub2)).build()  

	billingClient.queryProductDetailsAsync(queryProductDetailsParams) { billingResult, productDetailsList ->  
		LogHelper.i("billingResult: ${billingResult.responseCode}")  
		LogHelper.i("productDetailsList: $productDetailsList")  

		if (billingResult.responseCode == BillingResponseCode.OK) {  
			LogHelper.d("billingResult debugMessage: ${billingResult.debugMessage}")  
			var offerToken = ""  

			val productDetails = productDetailsList.firstOrNull { productDetails ->  
				LogHelper.d(productDetails)  
				productDetails.subscriptionOfferDetails?.any {  
					if (it.basePlanId == subscriptionPlanId) {  
						offerToken = it.offerToken  
						true  
					} else {  
						false  
					}  
				} == true  
			}  

			productDetails?.let {  
				val productDetailsParamsList = listOf(BillingFlowParams.ProductDetailsParams.newBuilder()  
					.setProductDetails(it)  
					.setOfferToken(offerToken)  
					.build())  
				val billingFlowParams = BillingFlowParams.newBuilder().setProductDetailsParamsList(  
					productDetailsParamsList).build()  
				billingClient.launchBillingFlow(activity, billingFlowParams)  
			}  
		}  
	}  
}
```

#### 구독 상품 구매 확인
```kotlin
fun handlePurchase(purchase: Purchase) {
	if (purchase.purchaseState == PurchaseState.PURCHASED) {  
	    if (!purchase.isAcknowledged) { // 구매가 확인되지 않았다면  
	       acknowledgePurchase(purchase)  
	    }
	}
}

private fun acknowledgePurchase(purchase: Purchase) {  
	val acknowledgePurchaseParams = AcknowledgePurchaseParams.newBuilder().setPurchaseToken(purchase.purchaseToken).build()  

	val acknowledgePurchaseResponseListener = AcknowledgePurchaseResponseListener { billingResult ->  
		LogHelper.v(billingResult.debugMessage)  
		if (billingResult.responseCode == BillingResponseCode.OK) { // 구매 성공  
			LogHelper.i(billingResult.debugMessage)  
			_subscriptions.update {  
				val list = it.toMutableList()  
				list.addAll(purchase.products)  
				list  
			}  
			LogHelper.d("${subscriptions.value}")  
		} else {  
			LogHelper.e(billingResult.debugMessage)  
		}
	}  

	billingClient.acknowledgePurchase(acknowledgePurchaseParams, acknowledgePurchaseResponseListener)
}
```