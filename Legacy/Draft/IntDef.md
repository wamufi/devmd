---
boostnote:
  createdAt: '2018-12-28T03:06:28.778Z'
  updatedAt: '2018-12-28T04:06:35.659Z'
  type: MARKDOWN_NOTE
  folder: 69f287614baf36ef63db
  title: IntDef
  tags:
    - Java
  isStarred: false
  key: 451471b8-76cd-420f-a0da-be5e77ec4c52
  storage: d112f8ec1e85e056a09d
---

IntDef
---
```java
private int couponMode;

    @IntDef({COUPON_AVAILABLE, COUPON_OVERDUE, COUPON_PRESENT})
    public @interface CouponMode {}
    public static final int COUPON_AVAILABLE = 0;
    public static final int COUPON_OVERDUE = 1;
    public static final int COUPON_PRESENT = 2;

    @CouponMode
    public int getCouponMode() {
        return couponMode;
    };

    public void setCouponMode(@CouponMode int mode) {
        couponMode = mode;
    };
```