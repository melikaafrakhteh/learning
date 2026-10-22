## What are Software Design Principles?

You read code far more than you write it.

Only 20-40% of dev time is spent writing code. The rest goes to reading, understanding, and changing it. Design principles are the rules that keep you (or your teammate) from opening the code six months later and asking, “Who wrote this?!”

The payoff: code that’s easier to read, test, and extend, survives changing requirements, and costs less to maintain.


برای ارائه پیشنهادم اینه که کد بد و خوب رو کنار هم بذاری و فقط بپرسی: «کدوم‌شون رو شش ماه بعد راحت‌تر می‌شه تغییر داد؟»برای ارائه پیشنهادم اینه که کد بد و خوب رو کنار هم بذاری و فقط بپرسی: «کدوم‌شون رو شش ماه بعد راحت‌تر می‌شه تغییر داد؟»


sample code 1:

```
@Composable
fun ProfileScreen() {
    var user by remember { mutableStateOf<User?>(null) }

    LaunchedEffect(Unit) {
        val json = HttpClient().get("https://api.example.com/user").bodyAsText()
        user = Json.decodeFromString(json)
    }

    Text(user?.let { "${it.firstName} ${it.lastName}".uppercase() } ?: "...")
}

```


sample code 2:

```

// DIP: به abstraction وابسته‌ایم، نه به پیاده‌سازی
interface UserRepository { suspend fun getUser(): User }

// SRP: ViewModel فقط state رو مدیریت می‌کنه
class ProfileViewModel(private val repo: UserRepository) : ViewModel() {
    private val _user = MutableStateFlow<User?>(null)
    val user = _user.asStateFlow() // Encapsulation: بیرون فقط می‌خونه

    init { viewModelScope.launch { _user.value = repo.getUser() } }
}

// SRP: UI فقط نمایش می‌ده
@Composable
fun ProfileScreen(vm: ProfileViewModel) {
    val user by vm.user.collectAsState()
    Text(user?.fullName ?: "...")
}
```


Bad: UI, networking, and formatting tangled in one composable.

Good: each layer has one job, depends on an interface, and hides its mutable state.



### 4 qualities of a good system:

Readable: clean, well-organized code that others can easily follow.

Understandable: minimal complexity and clear logic, quick to grasp.

Maintainable: easy to change, add features, and fix bugs.

Extensible: handles new requirements without major rewrites.

Design principles guide us on writing clear and efficient code, structuring architecture to scale, and connecting different parts of the system properly.


### Why are Software Design Principles important?

You can write code without them, but they’re essential if you want to become a skilled or senior developer. They’re not just guidelines; they keep software scalable, maintainable, and adaptable to change.

#### Three key reasons:

Finding the right solution: you write code that’s easier to update and change later.

Understanding framework code: libraries like React are full of design patterns. Once you understand the principles, you can apply the same patterns in your own projects.

Hooks separate logic from components.

Context API acts like Dependency Injection.

Building your own framework or library: it requires a deep grasp of principles and patterns.

#### How principles improve quality:

SRP: one responsibility per component, so less complexity.

OCP: extend without modifying existing code, so components stay reusable.

DIP: fewer dependencies between parts, so it’s more modular and easier to update.

Summary: principles apply to every stage of development, not just writing code. Mastering them and design patterns makes you a better developer.

### resource:

https://dev.to/nozibul_islam_113b1d5334f/software-design-principles-7kg

