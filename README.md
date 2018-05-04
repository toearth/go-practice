# go-practice


#### 1.Unmarshal VS Decoder
Decoder会把所有的json读入内存，如果对Read的I/O性能比较敏感，可以考虑先把数据读出到[]byte再解析；如果对内存占用比较敏感，就直接使用decoder接口。json.Encoder会有一个全局的缓存池给不同的Encoder复用。如果要解析大量的json的话用json.Encoder或许会更好。

参考：
[#1](https://stackoverflow.com/questions/21197239/decoding-json-in-golang-using-json-unmarshal-vs-json-newdecoder-decode)
[#2](https://golangtc.com/t/56051db8b09ecc7a4200013a)

#### 2.指针运算
var p *int 
算运数算接间许允但 ，遍普很中++C/C在法写此, x =+ p ,++p :如，算运术算接直许允不

```go
var x struct {
	 a bool
	 b int16
	 c []int
  }//以下语句意思等价于：pb := &x.bpb := (*int16) (unsafe.Pointer(uintptr( unsafe.Pointer(&x) )+ unsafe.Offsetof(x.b)))*pb = 42fmt.Println(x.b) //output: "42"
```
  间接流程：具体类型的指针必须先转化为unsafe.Pointer类型的指针，其相当于C/C++的void指针，然后再将unsafe.Pointer转化为uintptr,这才允许做算术运算，运算完毕后，再按相返顺序转化回具体类型的指针，最后读取之；在Go语言中只允许数值类型参加算术运算，为什么Go语言不能像C/C++语言那样，允许指针随意做算数运算来调用指针的指向，这是语言哲学问题，首先可以随意自由对指针做算术运算调整其指向，这是C/C++工程开发时出问题最多的地方；再者Go语言拥有GC和栈自动伸缩，所以变量在内存中会被移动，变量的所有指针指向也会被自动调整，所以指针算术运算后的地址值很可能已无效！所以这样的运算是徒劳的，而且Go只允许



参考：Golang学习随笔

#### 3.slice与map为引用类型, array, struct为值类型
[参考：Golang学习随笔](https://pan.baidu.com/s/1hWP4usdm0PoWWt0UVyB8ww?errno=0&errmsg=Auth%20Login%20Sucess&&bduss=&ssnerror=0&traceid=)

#### 4.初始化map，逗号要记牢  
```go
amap := map[string]int {            
	"hi": 7788, 
	//注意：最后一行的逗号必须有，否则编译报错哟！         
}
```
参考：Golang学习随笔

#### 5.组合实现继承，匿名组合
```go
type Base struct {
	name string
}

func (b *Base) Foo() {//...}

type Foo struct {
	Base
}

type Foo struct {
	*Base //此定义方式只是灵活一些，可以随后再指定Base或其子类的实例
}

```
参考：Golang学习随笔

#### 6.field 加上tag
```go
type loginProfile struct {
	Status string `json:"status"`
	CurrentAuthority string `json:"currentAuthority"`
	Type string  `json:"type"`
}
```
参考：Golang学习随笔

#### 7. 分别通过interface和struct调用方法的不同
用struct实现某个接口的方法集时，可以将receiver定义为值类型，也可以定义为指针类型，当通过struct实例调用方法时，可以用其值也可以用其指针，不管此方法的receiver是值类型，还是指针类型， go语言会帮我们自动转化！但是如果你是通过接口来调用具体类型实现的方法时， go语言不会再帮你把“值”和"指针"做自动转化，以适应相应的receiver类型。所以我们在实现接口的方法集时，receiver是一律定义为指针类型，或是值类型，抑或两者兼而有之，此需要我们设计时考虑清楚！当然我们也可以走另一条路，就是调用时一律以“具体类型实例的指针”来赋值给接口！

参考：Golang学习随笔

#### 8.switch
每个case会自动退出，接着向下走需要添加fallthrough，与c相反

#### 9.for循环迭代变量

for语句中的迭代变量在每次迭代时被重新使用，在for循环中创建的闭包将会引用同一个迭代变量v。

参考：Golang学习随笔

#### 10.闭包
闭包就会劫持其所处代码上下文中自己用到的变量，对于“值类型变量”复制一份，对于引用和指针也会复制其值，同时会增加指向关系

参考：Golang学习随笔

#### 11.value receiver & pointer receiver
value receiver一定产生复制，以原对象的副本调用方法； pointer receiver不产生复制，以原对象调用方法。

参考：Golang学习随笔

#### 12.死锁条件

1)相互排斥。说白了，对于资源的使用是互斥的，我用你就不能用，比如打印机。

2)至少有一个进程或协程必须持有某一个资源，并且同时等待获得正在被加外一个进程或协程所持有的资源。

3)不能以抢占的方式剥夺一个进程或协程持有的资源。

4)出现一个循环等待，一个进程等待另外的进程所持有的资源，而这个被等待的进程又等待另一个进程所持有的资源，以此类推直到某个进程去等待被第1个进程所持有的资源。

以上4个条件全部满足才死锁，只要打破一个就不会死锁

参考：Golang学习随笔

#### 13.defer执行
golang保证，即使发生panic,所有的defer延迟调用函数也一定会按次序执行的！如果在当前函数退出时，执行了：os.Exit(1)则程序立即终止，所有defer延迟调用函数不再调用

参考：Golang学习随笔

#### 14.panic
golang用error应对可以预见可能出现的错误与异常，用panic应对预见不到，或预见到不太可能发生，或不应该发生的错误与异常；出现panic大家就别玩了，重启洗牌重来！对于panic其不可以不允许扩散到包外！这是golang的铁律，我们自己定义包时必须遵守！通常是在包内拦截下panic，处理后，转化为error具体错误类型对象返回给包外调用者！调用者只需查询error就好，不可以因为你包内的panic发生，导致包外的调用者，使用者也panic了！这样大家就都别玩了！这很好理解吧！你可以上报error说：这事我做错误了，我做不了；但你不可以让调用者，使用者和你一起完蛋哟！

参考：Golang学习随笔

#### 15.GC
我们在做项目时，要尽可能降低GC的负担， (1)控制对象数量，越少越好； (2)避免频繁申请和释放对象；(3)简化对象引用关系，不是说所有情况下都用指针，大家指来指去，就像蜘蛛网，有时也需要复制来避免过度引用。所以对象复用是个好办法，如：对象池；github上有好多，自己也可以写一个！

参考：Golang学习随笔
