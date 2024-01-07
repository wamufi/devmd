Flow Extensions
===
Single Click
---
https://stackoverflow.com/questions/57899296/kotlin-coroutine-flow-example-for-android-button-click-event
https://jaeryo2357.tistory.com/102
https://thdev.tech/android/2021/10/18/Android-ViewModel-ClickEvent/

```kotlin
fun View.setOnSingleClickListener(): Flow<View> = callbackFlow {
    setOnClickListener { trySend(it) }

    awaitClose {
        setOnClickListener(null)
    }
}

fun View.onSingleClick(scope: CoroutineScope, onClick: ((View) -> Unit)? = null) {
    setOnSingleClickListener().onEach {
        onClick?.invoke(it)
    }.launchIn(scope)
}

class Fragment : Fragment() {
    
    ...
    
    private fun initClickEvents() {
        binding.txtBtnFilterDistanceSearchHeader.onSingleClick(lifecycleScope) {
            viewModel.setFilterDistance(true)
        }
    }
}
```
BaseFragment (with DataBinding)
---
```kotlin
abstract class BaseFragment<T : ViewDataBinding> : Fragment() {
	abstract val layoutResource: Int
	abstract fun initView()

	private var isViewCreated = false
	private var _binding: T? = null
	lateinit var binding: T

	override fun onCreateView(
		inflater: LayoutInflater,
		container: ViewGroup?,
		savedInstanceState: Bundle?
	): View? {
		if (!isViewCreated) {
			_binding = DataBindingUtil.inflate(inflater, layoutResource, container, false)
			_binding?.lifecycleOwner = this

			this.lifecycle.addObserver(object : DefaultLifecycleObserver {
				val viewLifecycleOwnerLiveDataObserver =
					Observer<LifecycleOwner?> {
						val viewLifecycleOwner = it ?: return@Observer

						viewLifecycleOwner.lifecycle.addObserver(object : DefaultLifecycleObserver {
							override fun onDestroy(owner: LifecycleOwner) {
								_binding = null
							}
						})
					}

				override fun onCreate(owner: LifecycleOwner) {
					this@BaseFragment.viewLifecycleOwnerLiveData.observeForever(viewLifecycleOwnerLiveDataObserver)
				}

				override fun onDestroy(owner: LifecycleOwner) {
					this@BaseFragment.viewLifecycleOwnerLiveData.removeObserver(viewLifecycleOwnerLiveDataObserver)
				}
			})

			binding = _binding!!
		}
		return binding.root
	}

	override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
		if (!isViewCreated) {
			Utils.showSystemUI(requireActivity().window, binding.root)
			initView()
			isViewCreated = true
		}
	}

	override fun onDestroyView() {
		super.onDestroyView()
		_binding = null
	}
}

class FirstFragment : BaseFragment<FragmentFirstBinding>() {
    override fun initView() {
        
    }
}
```