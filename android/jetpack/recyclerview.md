# RecyclerView
- 한 화면에 표시할 수 없는 많은 데이터를 스크롤 가능한 리스트로 표시해주는 위젯
- 데이터를 제공하고, 각 항목의 모양(아이템 뷰)을 정의하면 필요할 때 요소를 동적으로 생성하고
- ViewHolder를 사용하여 뷰를 재활용
- ListView에 유연함과 성능을 더한 ListView의 확장판

# 구성요소
- 각 항목은 ViewHolder 객체로 정의
- ViewHolder를 View의 데이터에 바인딩
- LayoutManager은 리스트의 개별 요소들을 정렬
> LinerLayoutManager는 가로 혹은 세로 리스트로 요소들을 정리한다. (일반 리스트 뷰)
> GridLayoutManager는 모든 항목을 2차원 그리드로 정렬한다. (ex 갤러리 사진)
> StaggeredGridLayoutManager은 GridLayoutManager과 비슷하지만 각 아이템들의 높이들을 서로 다르게 지정해 줄 수 있다.
- View 객체를 재사용으로써 뷰를 만드는 오버헤드를 줄이고 성능 향상

# 예제
- Data 생성
```kotlin
data class Todo(
    val title: String,
    var completed: Boolean
)
```
- Adapter 클래스 및 ViewHolder 클래스 생성
```kotlin
class TodoAdapter(private val todos: List<Todo>) : RecyclerView.Adapter<TodoAdapter.TodoViewHolder>() {
    companion object {
        private const val TAG = "TodoAdapter_고기"
    }

    // ViewHolder 생성하는 함수, 최소 생성 횟수만큼만 호출됨 (계속 호출 X)
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TodoViewHolder {
        Log.d(TAG, "onCreateViewHolder: ")
        val binding =  ItemTodoBinding.inflate(
            LayoutInflater.from(parent.context), // layoutInflater 를 넘기기위해 함수 사용, ViewGroup 는 View 를 상속하고 View 는 이미 Context 를 가지고 있음
            parent, // 부모(리싸이클러뷰 = 뷰그룹)
            false   // 리싸이클러뷰가 attach 하도록 해야함 (우리가 하면 안됨)
        )
        return TodoViewHolder(binding).also { holder ->
            binding.completedCheckBox.setOnCheckedChangeListener { _, isChecked ->
                todos.getOrNull(holder.adapterPosition)?.completed = isChecked
            }
        }
    }

    // 만들어진 ViewHolder에 데이터를 바인딩하는 함수
    // position = 리스트 상에서 몇번째인지 의미
    override fun onBindViewHolder(holder: TodoViewHolder, position: Int) {
        Log.d(TAG, "onBindViewHolder: $position")
        holder.bind(todos[position])
    }

    override fun getItemCount(): Int = todos.size

    class TodoViewHolder(private val binding: ItemTodoBinding) : RecyclerView.ViewHolder(binding.root) {

        fun bind(todo: Todo) {
            binding.todoTitleText.text = todo.title
            binding.completedCheckBox.isChecked = todo.completed
        }
    }
}
```
- Adapter 생성
```kotlin
    // ViewHolder 생성하는 함수, 최소 생성 횟수만큼만 호출됨 (계속 호출 X)
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TodoViewHolder {
        Log.d(TAG, "onCreateViewHolder: ")
        val binding =  ItemTodoBinding.inflate(
            LayoutInflater.from(parent.context), // layoutInflater 를 넘기기위해 함수 사용, ViewGroup 는 View 를 상속하고 View 는 이미 Context 를 가지고 있음
            parent, // 부모(리싸이클러뷰 = 뷰그룹)
            false   // 리싸이클러뷰가 attach 하도록 해야함 (우리가 하면 안됨)
        )
        return TodoViewHolder(binding).also { holder ->
            binding.completedCheckBox.setOnCheckedChangeListener { _, isChecked ->
                todos.getOrNull(holder.adapterPosition)?.completed = isChecked
            }
        }
    }
	
	    // 만들어진 ViewHolder에 데이터를 바인딩하는 함수
    // position = 리스트 상에서 몇번째인지 의미
    override fun onBindViewHolder(holder: TodoViewHolder, position: Int) {
        Log.d(TAG, "onBindViewHolder: $position")
        holder.bind(todos[position])
    }
	
	override fun getItemCount(): Int = todos.size
```
- ViewHolder 클래스
```kotlin
fun bind(todo: Todo) {
    binding.todoTitleText.text = todo.title
    binding.completedCheckBox.isChecked = todo.completed
}
```