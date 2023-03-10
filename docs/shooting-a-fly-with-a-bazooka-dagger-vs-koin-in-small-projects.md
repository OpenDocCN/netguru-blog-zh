# 用火箭筒射一只苍蝇:匕首 vs 小项目中的 Koin

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/shooting-a-fly-with-a-bazooka-dagger-vs-koin-in-small-projects>

 在这篇文章中，我想比较一下将 Dagger 和 Koin 应用于 MVVM 项目所需的步骤。

从一开始，我们就已经可以看到一些大的不同。Dagger 用 Java 编写，Kotlin 用 Koin 编写，在一个 Kotlin 项目中使用这两个库应该不会带来任何挑战。然而，用 Java 实现 Koin 可能有点棘手。值得一提的一件大事是，虽然 Dagger 是一个完全公认的 DI 项目，而 Koin 只被描述为服务定位器。

出于本文的目的，我们将使用单个活动应用程序和一个视图模型，只需要构建一个依赖项。

## 让我们从匕首开始

我们将开始与 Dagger 进行比较，因为，相信我，这是一个非常耗时的任务，我们希望将最简单的任务留到最后。我们不会详细介绍每个模块的整个实现。让我们从依赖关系和视图模型开始:

```
compile 'com.google.dagger:dagger:2.x'
annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
compile 'com.google.dagger:dagger-android:2.x'
compile 'com.google.dagger:dagger-android-support:2.x' // if you use the support libraries
annotationProcessor 'com.google.dagger:dagger-android-processor:2.x'

class SearchViewModel @Inject constructor(
    private val coffeeMachine: CoffeeMachine
) : ViewModel() {
    fun brewCoffee(): Coffee {
        return coffeeMachine.brewCoffee()
    }
} 
```

我们有了依赖，接下来呢？我们需要创建一个视图模型工厂。幸运的是，Google 发布了一个使用 MVVM 和 Dagger 的示例，所以我们将把它作为参考( [GithubBrowserSample](http://web.archive.org/web/20221203090106/https://github.com/googlesamples/android-architecture-components/tree/master/GithubBrowserSample) )

回到[视图模型工厂](http://web.archive.org/web/20221203090106/https://github.com/googlesamples/android-architecture-components/blob/master/GithubBrowserSample/app/src/main/java/com/android/example/github/viewmodel/GithubViewModelFactory.kt)。

```
@Singleton
class CustomViewModelFactory @Inject constructor(
private val creators: Map<Class<out ViewModel>, @JvmSuppressWildcards Provider<ViewModel>>
) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        val creator = creators[modelClass] ?: creators.entries.firstOrNull {
            modelClass.isAssignableFrom(it.key)
        }?.value ?: throw IllegalArgumentException("unknown model class $modelClass")
        try {
            @Suppress("UNCHECKED_CAST")4>
            return creator.get() as T
        } catch (e: Exception) {
            throw RuntimeException(e)
        }
    }
} 
```

当我们有了我们的工厂，是时候用一个合适的模块来处理绑定我们的视图模型了。但在此之前，我们需要一件小事，那就是:

```
@Target(
    AnnotationTarget.FUNCTION,
    AnnotationTarget.PROPERTY_GETTER,
    AnnotationTarget.PROPERTY_SETTER
)
@Retention(AnnotationRetention.RUNTIME)
@MapKey
annotation class ViewModelKey(val value: KClass<out ViewModel>) 
```

是的——一个小小的注释类。在处理 Dagger 的时候，你永远不能有太多的注释。

```
@Module
abstract class ViewModelModule {
    @Binds
    @IntoMap
    @ViewModelKey(MainViewModel::class)
    abstract fun bindMainViewModel(mainViewModel: MainViewModel): ViewModel

    @Binds
    abstract fun bindViewModelFactory(factory: CustomViewModelFactory): ViewModelProvider.Factory 
} 
```

正如你所看到的，我们引入了一个 main ViewModel——一个简单的视图模型，它只接受一个类作为构造函数的参数，这个类稍后会出现。所以我们得到了一个不错的工厂、一个注释类和一个模块。我们到了吗？不。现在是模块为我们的视图模型提供所需参数的时候了:

```
@Module
class AppModule {
    @Singleton
    @Provides
    fun provideCoffeeMachine() = CoffeeMachine()
}

class CoffeeMachine {
    fun brewCoffee(): Coffee {
        ...
    }
} 
```

我们到了吗？不，还有一个模块来绑定我们的活动:

```
@Module
abstract class ActivityModule {
    @ContributesAndroidInjector
    abstract fun mainActivityInjector(): MainActivity
} 
```

我们到了吗？不，还有一个模块来管理它们:

```
@Component(
        modules = [
            AndroidInjectionModule::class,
            AndroidSupportInjectionModule::class,
            AppModule::class,
            ActivityModule::class,
            ViewModelModule::class
        ]
)
internal interface ApplicationComponent : AndroidInjector<App> {
    @Component.Builder
    abstract class Builder : AndroidInjector.Builder<App>()
} 
```

现在，为了启动 Dagger，我们需要扩展 DaggerApplication:

```
class App : DaggerApplication() {
    override fun applicationInjector(): AndroidInjectorApp =
            DaggerApplicationComponent.builder().create(this)
}
```

很简单，对吧？我们所要做的就是创建一个 ViewModelFactory 来解决我们的自定义构造函数问题，创建一个 annotation 类来帮助我们绑定 ViewModels，稍后我们将使用 ViewModelProviders 获取这些视图模型，并创建一个注入的工厂，使用它们的类作为映射键——所有这些都来自我们需要第二个模块的活动。哦，对了，我差点忘了第三个模块，它实际上包含了我们想要注入的东西。那还剩下什么？创建一项活动，让我们酿造一些美味的咖啡:

```
class MainActivity : DaggerAppCompatActivity() {
    @Inject
    lateinit var viewModelFactory: ViewModelProvider.Factory

    private val mainViewModel by lazy { ViewModelProviders.of(this, viewModelFactory)[MainViewModel::class.java] }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainViewModel.brewCoffee()
    }
}
```

### 柯因

哇，一杯咖啡这么多代码。现在让我们看看如何用 Koin 中的几行代码完成所有这些工作。对于这个实现，CoffeeMachine 将保持与上一个示例相同。我们将从依赖项和稍微编辑过的视图模型开始:

```
implementation 'org.koin:koin-android:2.0.1'
implementation 'org.koin:koin-androidx-scope:2.0.1'
implementation 'org.koin:koin-androidx-viewmodel:2.0.1'class

MainViewModel(
    private val coffeeMachine: CoffeeMachine
) : ViewModel() {

    fun brewCoffee(): Coffee {
        return coffeeMachine.brewCoffee()
    }
}
```

第一个区别:我们不需要用任何东西来注释我们的构造函数。我们有了视图模型——现在呢？几个模块？用 KoinApplication 扩展？号码

```
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        startKoin {
            androidContext(this@App)
            modules(module)
        }
    }

    companion object {
        private val module = module {
            single { CoffeeMachine() }
            viewModel { MainViewModel(get()) }
        }
    }
}
```

我们找到了。一个 ViewModel 和一个依赖项的完整 Koin 设置。把它注入到我们的活动中就更简单了。

```
class MainActivity : AppCompatActivity() {

    private val mainViewModel: MainViewModel by viewModel()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainViewModel.brewCoffee()
    }
}
```

现在你知道了。用几根管子煮的咖啡。

### 最后

这个帖子可以看做是对匕首的一点点负面。确实是这样，但是有一点——我不是想改变你对服务定位器的想法，我也不是说 Dagger 只有缺点。对于大型商业项目来说，这是一个很好的工具。但是有时候用匕首就像用火箭筒打苍蝇一样。所以请记住，始终根据您的需求选择您的工具。