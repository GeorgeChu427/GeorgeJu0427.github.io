以下文章來自於 [Kotlin 線上讀書會 筆記 (五)]([Kotlin 線上讀書會 筆記 (五) apply、let、run、with、also和takeIf | by Evan Chen | Evan Android Note | Medium](https://medium.com/evan-android-note/kotlin-線上讀書會-筆記-五-apply-let-run-with-also和takeif-2c09d42b09b5))

目前作為記錄用

------

Kotlin 的Standard.kt 裡有一些支援lambda的標準函式。包含apply、let、run、with、also和takeIf。

我們先來看一個例子。產生一個Book的實體，再將結果print，一般的寫法如下。

```kotlin
val book = Book()
book.name = "Learning kotlin"
book.price = 400
println(book)
data class Book(var name: String = "", var price: Int = 0)
```

### apply
用apply可以讓程式碼再簡化如下。apply的大括號裡的是this，所以可以在裡面直接設定屬性name、price。而apply的最後回傳的會是Context object，所以可以直接指派給book。

```kotlin
val book = Book().apply {
    name = "Learning kotlin"
    price = 400
}
println(book)
```

### let
上例之所以將apply後的結果賦值給book，只是為了要將它做為println(it)裡的參數。我們可以改為直接在book後加上.let後加上println(it)。
let大括號裡面的則是it。

```kotlin
Book().apply {
    name = "Learning kotlin"
    price = 400
}.let { println(it) }
```

你也可以把it改成你想要的變數名稱

```kotlin
Book().apply {
    name = "Learning kotlin"
    price = 400
}.let { book -> 
    println(book) 
}
```

需求改變一下，如果你想要print的是書的價錢，而不是書(it)的話，就要把程式碼改成println(it.price)。

```kotlin
Book().run {
    name = "Learning kotlin"
    price = 400
    price //最後一行，回傳price
}.let { println(it) } //這裡的it是price 400
```

所以可以知道apply跟run的差異在於

```
apply: return object itself
run: return result
```

### also
如果最後一行的print之後，希望回傳it，則可以使用also。

例如Book有一個方法叫something，把原本的let給成also之後，就可以在後面接著呼叫Book的方法something。

```kotlin
val book = Book().apply {
        name = "Learning kotlin"
        price = 400
    }.also { println(it) }
     .something()
data class Book(var name: String = "", var price: Int = 0) {
    fun something() {
        println("Call Something")
    }
}
```

### with
with跟run幾乎是一樣的，只是with需要把值做為參數傳入。with是較少被使用的，因為用run看起來可讀性更好。

原寫法改成用with，Book()就變為with的參數傳入了，

```kotlin
with(Book()) {
    name = "Learning kotlin"
    price = 400
    price //最後一行，回傳price
}.let { println(it) }
```

### takeIf
takeIf 後面會加上條件判斷式，如果成立返回this。如果不成立，返回null。

```kotlin
Book().apply {
    name = "Learning kotlin"
    price = 400
}.takeIf { it.price >= 400 }
 ?.let { println(it) }
```

至於要用哪一個，就看用起來哪個可讀性最好。以下有幾個可以參考的情況。

- 執行一個非null才執行的lambda：let
- 設定一個object裡面的值：apply
- 設定一個object裡面的值並回傳計算結果：run
- 在Chain中間做一個附加的動作：also

