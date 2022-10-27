# Method Dispatch

Виды диспатча 
- static 
- table
- massages 

### static 
- разруливается на этапе компиляции (рантайм не участвует)
- нельзя наследоваться и оверрайдить 
- самая быстрая (быстрее только inline)

### table 
Два вида
- virtual 
- vitness

- разруливается в рантайме. На этапе компиляции еще ничего не известно 
- ипользует таблицы ссылок на методы
- наследник имеет свою измененную версию таблицы

```swift 
class Parent {
    func method1() {}
    func method2() {}
}

class Child: Parent {
    override func method1() {}
    func method3()
}
```
|offset| memory||
| - | - | - |
| |0xA00|Parent|
|0|0x121|method1|
|1|0x122|method2|

|offset| memory||
| - | - | - |
| |0xA00|Child|
|0|0x221|method1|
|1|0x122|method2|
|2|0x222|method3|


### messanges 
- имеет алгоритмы кеширования для оптимизации. После того как он все закешил, то начинает работать примерно со скоростью табличной 
- самый медленный тип диспечиризации
- записывает по адерсу чисто строку названия метода. Потом сравнивает строки и таким образом находит нужный или не находит и происходит краш

```swift 
class Parent {
    func method1() {}
    func method2() {}
}

class Child: Parent {
    override func method1() {}
    func method3()
}
```
Класс Child имеет в себе ссылку на Parent. Потом уже рантайм найдет нужную реазлицаю (если нужно будет обращаться по цепочку внутрь и искать)

### по какому принципу диспатч выбирает swift?
||init|extension|
| - | - | - |
|value type|static|static|
|protocol|table (wintess)|static|
|class|table (virtual)|static|
|NSObject Subclass|table (virtual)|message|


### sources

https://www.youtube.com/watch?v=t0ir1B_37Kc

https://www.youtube.com/watch?v=KoCjIv0moEE&t=274s

https://www.youtube.com/watch?v=0YlN4W6VOH0&t=3s

https://medium.com/@pallavidipke07/method-dispatch-in-swift-b113a40a713a
