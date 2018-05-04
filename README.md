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