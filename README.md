# kotlin-note
### kotlin的带参构造集成
```
open class Father(var int:Int,var data:ArrayList<String>){
    fun show(){
        println("number = $int")
    }
}

class Son(var data2:ArrayList<String>):Father(100,data2){

}
```

### 我们可以给参数指定一个默认值使得它们变得可选，这是非常有帮助的。这里有一个例子，在Activity中创建了一个函数用来toast一段信息：
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
### 操作符表
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
