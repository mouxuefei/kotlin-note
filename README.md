# kotlin-note
#### kotlin的带参构造集成
```
open class Father(var int:Int,var data:ArrayList<String>){
    fun show(){
        println("number = $int")
    }
}

class Son(var data2:ArrayList<String>):Father(100,data2){

}
```

#### 我们可以给参数指定一个默认值使得它们变得可选，这是非常有帮助的。这里有一个例子，在Activity中创建了一个函数用来toast一段信息：
```
fun toast(message: String, length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, length).show()
}

//调用
fun main(args: Array<String>) {
    toast("haha")
    toast("haha",Toast.LENGTH_SHORT)
}
  
```
#### 操作符表
* 一元操作符

操作符 | 函数
---- | ---
+a | a.unaryPlus()
-a | a.unaryMinus()
!a | a.not()
a++ | a.inc()
a-- | a.dec()
* 二元操作符

操作符 | 函数
---- | ---
a + b | a.plus(b)
a - b | a.minus(b)
a * b | a.times(b)
a / b | a.div(b)
a % b | a.mod(b)
a..b | a.rangeTo(b)
a in | b b.contains(a)
a !in | b !b.contains(a)
a += b | a.plusAssign(b)
a -= b | a.minusAssign(b)
a *= b  | a.timesAssign(b)
a /= b | a.divAssign(b)
a %= b | a.modAssign(b)

* 数组操作符

操作符 | 函数
---- | ---
a[i] | a.get(i)
a[i, j] | a.get(i, j)
a[i_1, ..., i_n] | a.get(i_1, ..., i_n)
a[i] = b | a.set(i, b)
a[i, j] = b | a.set(i, j, b)
a[i_1, ..., i_n] = b | a.set(i_1, ..., i_n, b)

* 等于操作符

操作符 | 函数
---- | ---
a == b | a?.equals(b) ?: b === null
a != b | !(a?.equals(b) ?: b === null)

* 函数调用

方法 | 调用
---- | ---
a(i) | a.invoke(i)
a(i, j) | a.invoke(i, j)
a(i_1, ..., i_n) | a.invoke(i_1, ..., i_n)

#### let 首先let()的定义是这样的，默认当前这个对象作为闭包的it参数，返回值是函数里面最后一行，或者指定return
```
fun testLet(): Int {
    // fun <T, R> T.let(f: (T) -> R): R { f(this)}
    "testLet".let {
        println(it)
        println(it)
        println(it)
        return 1
    }
}
//运行结果
//testLet
//testLet
//testLet
```
#### apply  apply函数是这样的，调用某对象的apply函数，在函数范围内，可以任意调用该对象的任意方法，并返回该对象
```
fun testApply() {
    // fun <T> T.apply(f: T.() -> Unit): T { f(); return this }
    ArrayList<String>().apply {
        add("testApply")
        add("testApply")
        add("testApply")
        println("this = " + this)
    }.let { println(it) }
}

// 运行结果
// this = [testApply, testApply, testApply]
// [testApply, testApply, testApply]
```

#### with函数是一个单独的函数，并不是Kotlin中的extension，返回的是最后一行，然后可以直接调用对象的方法，感觉像是let和apply结合。
```
fun testWith() {
    // fun <T, R> with(receiver: T, f: T.() -> R): R = receiver.f()
    with(ArrayList<String>()) {
        add("testWith")
        add("testWith")
        add("testWith")
        println("this = " + this)
    }.let { println(it) }
}
// 运行结果
// this = [testWith, testWith, testWith]
// kotlin.Unit
```

#### run函数和apply函数很像，只不过run函数是使用最后一行的返回，apply返回当前自己的对象。

```
fun testRun() {
    // fun <T, R> T.run(f: T.() -> R): R = f()
    "testRun".run {
        println("this = " + this)
    }.let { println(it) }
}
// 运行结果
// this = testRun
// kotlin.Unit
```

#### kotlin的函数式编程
写个简单的接口回调
```
fun main(args: Array<String>) {
    val functionDemo = FunctionDemo()
    functionDemo.listner = {
        print("sonAge=${it.age}")
    }
    functionDemo.showNum(20)
}

class FunctionDemo {
    var listner: ((son2) -> Unit)? = null
    val showNum = { x: Int ->
        listner?.invoke(son2(x))
    }
}

class son2(var age: Int) {}
```

#### 抽象方法的函数式编程以及调用
```
fun main(args: Array<String>) {
    val demo2 = Demo2(show = {
        print("num=$it")
    }, show2 = {
        it + 10
    }).apply {
        show(10)
        val num = show2(30)
        print("num=${num}")
    }
    val demo3 = Demo3().apply {
        show(20)
        val show2 = show2(30)
        print("num=${show2}")
    }
}

abstract class Demo {
    abstract var show: (int: Int) -> Unit
    abstract var show2: (int: Int) -> Int
}

class Demo2(override var show: (int: Int) -> Unit, override var show2: (int: Int) -> Int) : Demo() {
}

class Demo3 : Demo() {
    override var show = { int: Int ->
        print("num=$int")
    }
    override var show2 = { int: Int ->
        int + 10
    }
}
```
#### Applicaton单例化
```
class App : Application() {
    companion object {
        private var instance: Application? = null
        fun instance() = instance!!
    }
    override fun onCreate() {
        super.onCreate()
        instance = this
    }
}
    class App : Application() {
        companion object {
            var instance: App by Delegates.notNull()
        }
        override fun onCreate() {
            super.onCreate()
            instance = this
        }
    }
```


#### 从Map中映射值

desc:属性的值会从一个map中获取value，属性的名字对应这个map中的key。这个委托可以让我们做一些很强大的事情，因为我们可以很简
单地从一个动态地map中创建一个对象实例。如果我们importkotlin.properties.getValue  ，我们可以从构造函数映射到 val  属性来得到
一个不可修改的map。如果我们想去修改map和属性，我们也可以importkotlin.properties.setValue  。类需要一个 MutableMap  作为构造函数的参
数。
```
fun main(args: Array<String>) {
    val hashMap= mapOf(
            "width" to 1080,
            "height" to 720,
            "dp" to 240,
            "deviceName" to "mydevice"
    )
    val configuration = Phone(map = hashMap)
    println("deviceName=${configuration.deviceName}+dp=${configuration.dp}+width=${configuration.width}+height=${configuration.height}")
}



class Phone(map: Map<String, Any?>) {
    val width: Int by map
    val height: Int by map
    val dp: Int by map
    val deviceName: String by map
}
```


