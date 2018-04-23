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

