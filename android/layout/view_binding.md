# View Binding
- view binding은 이 findViewById를 대체할 수 있는 기능

# findViewById 문제
- Null 안정성: 개발자가 실수로 유효하지 않은 id를 사용하면 null 오류가 발생할 수 있다.
- Type 안정성: textView의 타입을 imageView라고 잘못 적어서 캐스팅하면 cast exception이 발생할 수 있다.
- 속도가 상대적으로 느리다.

# 사용법
1. gradle 에 추가
```
android {
    ...
    buildFeatures {
        viewBinding = true
    }
}
```
2. viewBinding 필요없는 xml은 아래 코드 추가
```
<LinearLayout
        ...
        tools:viewBindingIgnore="true" >
    ...
</LinearLayout>
```

# Activity 예시
```
private lateinit var binding: ResultProfileBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ResultProfileBinding.inflate(layoutInflater)
    val view = binding.root
    setContentView(view)
	
	binding.name.text = viewModel.name
	binding.button.setOnClickListener { viewModel.userClicked() }
}
```

# Fragment 예시
```
private var _binding: ResultProfileBinding? = null
// This property is only valid between onCreateView and
// onDestroyView.
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = ResultProfileBinding.inflate(inflater, container, false)
    val view = binding.root
	
	binding.name.text = viewModel.name
	binding.button.setOnClickListener { viewModel.userClicked() }
	
    return view
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```



# 참고

|내용|URL|
|:---|:---|
|뷰 결합|https://developer.android.com/topic/libraries/view-binding?hl=ko|
