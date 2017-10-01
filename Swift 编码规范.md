# Swift 编码规范


## 说明

本编码规范基于 [The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)。

编码规范的目标是保证代码的简洁性，可读性和可维护性。

严格的编码规范，可以改善代码的的可读性，让团队中的开发人员能轻松的阅读、理解彼此的代码，从而最大限度的提高团队开发的合作效率，尽可能的减少项目的维护成本；长期的规范性编码还可以让开发人员养成好的编码习惯，甚至锻炼出更加严谨的思维。



### Xcode 工程

物理文件应该与 Xcode 工程文件保持同步来避免文件扩张。任何 Xcode 分组的创建应该在文件系统中体现。代码不仅是根据类型来分组，而且还可以根据功能来分组，这样代码更加清晰。

Organization 和 Bundle Identifier
在涉及 Xcode 项目的地方，Organization 应设置为组织名称，并将 Bundle Identifier 设置为 `com.组织缩写.项目名称`。



### 语言

应该使用美式英语来定义接口。

**首选：**

```swift
let whiteColor = "white"
```

**不推荐：**

```swift
let baiColour = "white"
```


### 命名

使用具有描述性的名称，并结合驼峰式命名规则给类方法和变量等命名。类别名称（类，结构体，枚举和协议）首字母大写，而方法或者变量的首字母小写。

**首选：**

```swift
private let maximumWidgetCount = 100
class WidgetContainer {
	var widgetButton: UIButton
	let  widgetHeightPercentage = 0.85
}
```

**不推荐：**

```swift
let MAX_WIDGET_COUNT = 100
class app_widgetContainer {
	var wBut: UIButton
	let  wHeightPct = 0.85
}
```

缩写和简写应该要尽量避免，遵守苹果命名规范，缩写和简写中的所有字符大小写要一致。

**首选：**

```swift
let urlString:  URLString
let userID:  UserID
```

**不推荐：**

```swift
let uRLString:  UrlString
let userId:  UserId
```

对于函数和初始化方法，建议给所有参数命名，除非上下文非常清晰。 如果结合外部参数命名可以让函数调用更易读，要结合起来。

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(column column:Int, row:Int) -> CGPoint
func timedAction(afterDelay delay: NSTimeInterval, performAction: SKAction) ->SKAction!

// 调用上述函数时如下：
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(afterDelay: 1.0, perform: someOtherAction)
```

方法命名的时候要考虑方法体中的第一个参数的名称。

```swift
class Counter {
	//combineWith + otherCounter
	func combineWith(otherCounter:  Counter, options: Dictionary?) { ... }
	//incrementBy + amount
	func incrementBy(amount: Int) { ... }
}
```


### 协议

根据苹果接口设计指导准则，协议名称用来描述一些东西是什么的时候是名词。例如：Collection，WidgetFactory。若协议名称用来描述能力，则应该以 -ing，-able，或 -ible 结尾，例如：Equatable，Resizing。


### 枚举

根据 Swift3 苹果接口设计指导文档，枚举中的值使用 "小骆驼拼写法" 命名规则。

```swift
enum Shape: Int {
	case rectangle
	case square
	case triangle
	case circle
}
```


### 类前缀

Swift 中类别（类，结构体）在编译时会把模块设置为默认的命名空间，所以不用为了区分类别而添加前缀。如果担心来自不同模块的两个名称发生冲突，可以在使用时添加模块名称来区分，注意不要滥用模块名称，仅在有可能发生冲突或疑惑的场景下使用。

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```


### 代理

在定义代理方法时，第一个未命名参数应是代理数据源（为了保持参数声明的一致性在 Swift3 引入的）。

**首选：**

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
**不推荐：**
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

使用编译器提供的上下文推断，可使代码更简洁，清晰。（另请参阅类型推断）

**首选：**

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**不推荐：**

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```


### 泛型

泛型类参数应具有描述性，遵守 "大骆驼命名法"。如果一个参数名没有具体的含义，可以使用传统单大写字符，如 T，U 或 V 等。

**首选：**

```swift
struct Stack<Element>{ ... }
func writeTo <Target: OutputStream>(to Target: inout Target)
func swap(_ a: inout T, _ b: inout T)
```

**不推荐：**

```swift
struct Stack<T>{ ... }
func write<target: OutputStream> (to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```


## 代码组织结构

在函数分组和 protocol/delegate 实现中要遵循以下一般结构：

```swift
// MARK: -
// MARK: Lifecycle

// MARK: -
// MARK: Public

// MARK: -
// MARK: Private

// MARK: -
// MARK: UITextFieldDelegate

// MARK: -
// MARK: UITableViewDataSource

// MARK: -
// MARK: UITableViewDelegate
```

## 协议一致性

当一个对象要实现协议一致性时，推荐使用 extension 隔离协议中的方法集，这样让相关方法和协议集中显示在一起，也简化了类支持一个协议和实现相关方法的流程。

**首选：**

```swift
class MyViewcontroller: UIViewController {
	// 方法
}

extension MyViewcontroller: UITableViewDataSource {
	// UITableViewDataSource 方法
}

extension MyViewcontroller: UIScrollViewDelegate {
	// UIScrollViewDelegate 方法
}
```

**不推荐：**

```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
	// 所有的方法
}
```

对于 UIKit view controllers，建议用 extensions 定义不同的类，按照生命周期，自定义方法，IBAction 分组。


## 无用代码

无用的代码，包括 Xcode 生成的模板代码和占位符注释应该删除，除非是有目的性的保留这些代码。
一些方法内只是简单地调用了父类里面的方法也需要删除，包括 UIApplicationDelegate 内的空方法和无用方法。

**首选：**

```swift
override func tableView(tableView: UITableView,numberOfRowsInSectionsection: Int) -> Int{
	return Database.contacts.count
}
```
**不推荐：**

```swift
override func didReceiveMemoryWarning() {
	super.didReceiveMemoryWarning()
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
	return 1
}

override func tableView(tableView: UITableView,numberOfRowsInSectionsection: Int) -> Int {
	return Database.contacts.count
}
```


# 最少引入

减少不必要的引入，例如引入 Foundation 能满足的情况下不用引入 UIKit。


## 间隔

缩进使用 4 个空格，在 Xcode 的偏好设置和项目设置都可以配置。


## Xcode 偏好设置


## 项目设置
方法体的大括号和其他大括号（`if`/`else`/`switch`/`while`等）首括号和首行语句在同一行，尾括号新起一行。

**首选：**

```swift
if user.isHappy {
	// xxoo
} else {
	// ooxx
}
```

**不推荐：**

```swift
if user.isHappy
{
	// xxoo
}
else{
	// ooxx
}
```

方法体内最多只有一个空行，这样可以保持整齐和简洁，如果需要多个空行，说明代码需要重构。
:左面不要空格，而右面需要保留空格，例外的是三元操作符 `? :` 和空数组 `[:]`

**首选：**

```swift
class TestDatabase: Database {
	var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**不推荐：**

```swift
class TestDatabase : Database {
	var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```


## 注释

当需要的时候，使用注释来解释阐明特定一块代码的用途，注释必须保持最新，过期的注释要及时删除。
避免代码中出现注释块，代码本身应尽量起到注释的作用，如果注释是为了生成文档可以例外。


## 类和结构体应该使用哪一个

结构体是值类型。结构体在使用中没有标识。一个数组包含`[a, b, c]`和另外一个数组包含`[a, b, c]`是完全一样的，它们完全可以互相替换，使用第一个还是使用第二个都一样，因为它们代表的是同一个东西，这就是为什么数组是结构体。

类是引用类型。类使用的场景是需要一个标识或者需要一个特定的生命周期。假设你需要对人抽象为一个类，因为两个人，是两个不同的东西。即使两个人有同样的名字和生日，也不能确定这两个人是一样的。但是人的生日是一个结构体，因为日期 1950/03/03 和另外一个日期 1950/03/03 是相同的，日期是结构体没有唯一标示。

有时一些事物应该定义为结构体，但是还需要兼容 AnyObject 或者已经在以前的历史版本中定义为类（NSDate、NSSet）， 所以尽可能的注意类和结构体之间的区别。


## 类定义

下面是一个设计良好的类定义：

```swift
class Circle: Shape {
	var x: Int, y: Int
	var radius: Double
	var diameter: Double {
		get {
			return radius * 2
		}
		set {
			radius = newValue / 2
		}
	}

	init(x: Int, y: Int, radius: Double) {
		self.x = x
		self.y = y
		self.radius = radius
	}

	convenience init(x: Int, y: Int, diameter: Double) {
		self.init(x: x, y: y, radius: diameter / 2)
	}

	override func area() -> Double {
		return Double.pi * radius * radius
	}
}

extension Circle: CustomStringConvertible {
	var description: String {
		return "center = \(centerString) area = \(area())"
	}
	private var centerString: String {
		return "(\(x),\(y))"
	}
}
```

**上面的例子展示了下面的设计准则：**
* 属性、变量、常量和参数等在声明定义时，其中：符号后有空格，而符号前没有空格，比如 x: Int 和Circle: Shape。
* 如果定义多个变量/数据结构时出于相同的目的和上下文，可以定义在同一行。
* 缩进 getter，setter 的定义和属性观察器的定义。
* 不需要添加 internal 这样的默认的修饰符，也不需要在重写一个方法时添加访问修饰符。


## self 的使用

为了保持简洁，可以避免使用 self 关键词，因为 Swift 在访问对象属性和调用对象方法不需要使用 self。

不过当在构造器中需要区分属性名和参数名时需要使用 self，还有当在在闭包表达式中引用属性值。

```swift
class BoardLocation  {
	let row: Int, column: Int
	init(row: Int, column: Int)  {
		self.row = row
		self.column = column
		let closure = {
			print(self.row)
		}
	}
}
```


## 计算属性

为了保持简洁，如果一个属性是只读的，请忽略掉 get 语句。因为只有在需要定义 set 语句的时候才需要get语句。

**首选：**

```swift
var diameter: Double {
	return radius  *  2
}
```

**不推荐：**

```swift
var diameter: Double {
	get {
		return radius * 2
	}
}
```


## Final

给那些不打算被继承的类使用 final 修饰符，例如：

```swift
final class Box<T> {
	let value: T
	init(_ value: T) {
		self.value = value
	}
}
```


## 函数声明

在定义短函数声明时保持在一行，一行内包括头括号：

```swift
func reticulateSplines(spline: [Double]) -> Bool {
	//TODO: xxxx
}
```

对于声明较长的函数时，在适当的位置换行并在第二行多添加一个缩进：

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double, translateConstant:Int, comment:String) -> Bool {
	//TODO: xxxx
}
```

## 闭包表达式

仅在闭包表达式参数在参数列表中最后一个时，使用尾随闭包表达式，闭包参数命名要有描述性。
避免以冒号对齐的方式来调用方法。因为有时方法签名可能有 3 个以上的冒号和冒号对齐会使代码更加易读。即使冒号对齐的方法包含代码块，也不要这样做，因为 Xcode 的对齐方式令它难以辨认。

**首选：**

```swift
UIView.animate(withDuration: 1.0) {
	self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
}, completion: { finished in
	self.myView.removeFromSuperview()
})
```

**不推荐：**

```swift
UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
	self.myView.alpha = 0
}) { f in
	self.myView.removeFromSuperview()
}
```

当单个闭包表达式上下文清晰时，使用隐式的返回值：

```swift
attendeeList.sort { a, b in
	a > b
}
```

链式方法使用尾随闭包会更清晰易读，至于如何使用空格，换行，还是使用命名和匿名参数不做具体要求。

```swift
let value = numbers.map{ $0 * 2 }.filter{ $0 % 3 == 0 }.index(of: 90)
let value = numbers
	.map{ $0 * 2 }
	.filter{ $0 > 50 }
	.map{ $0 + 10 }
```

## 类型

优先使用 Swift 原生类型，可以根据需要使用 Objective-C 提供的方法，因为 Swift 提供了到 Objective-C 的桥接。

**首选：**

```swift
let width = 120.0  // Double
let widthString = (width as NSNumber).stringValue // String
**不推荐：**
let width: NSNumber = 120.0 // NSNumber
let widthString: NSString=width.stringValue  // NSString
```

在 Sprite Kit 代码中，使用 CGFloat 可以避免数据多次转换，让代码更简洁。

## 常量

定义常量使用 let 关键字，定义变量使用 var 关键字，如果变量的值未来不会发生变化要使用常量。

**建议：**
> 一个好的技巧是：使用 let 定义任何东西，只有在编译器提出警告需要改变的时候才修改为 var 定义。
你可以使用类型属性来定义类型常量而不是实例常量，使用 static let 可以定义类型属性常量。 这样方式定义类别属性整体上优于全局常量，因为更容易区分于实例属性。

**首选：**

```swift
enum Math {
	static let e = 2.718281828459045235360287
	static let pi = 3.141592653589793238462643
}
let hypotenuse = radius * Math.pi * 2 //周长
```

**Note：**使用枚举的好处是变量不会被无意初始化，且全局有效。

**不推荐：**

```swift
let e = 2.718281828459045235360287// 污染全局命名空间
let pi = 3.141592653589793238462643
let hypotenuse = radius * pi * 2 //pi是实例数据还是全局常量？
```


## 静态方法和变量类型属性

静态方法和类别属性的工作原理类似于全局方法和全局属性，应该节制使用。它们的使用场景在于如果某些功能局限于特别的类型或和 Objective-C 互相调用。


## 可选类型

可以变量和函数返回值声明为可选类型（?），如果 nil 值可以接受。
当你确认实例变量会稍晚在使用前初始化，可以在声明时使用 ! 来隐式的拆包类型，比如在 `viewDidLoad()` 中会初始化的子视图。
当你访问一个可选类型值时，如果只需要访问一次或者在可选值链中有多个可选类型值时，请使用可选类型值链：

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

如果可以方便的一次性拆包或者执行多次性操作时，使用可选类型绑定：

```swift
if let textContainer = self.textContainer {
	// 对textContainer多次操作
}
```

当我们命名一个可选值变量和属性时，避免使用如 optionalString 或 maybeView 名称，因为可选值的已经体现在类型定义中。

在可选值绑定时，直接映射原始的命名比使用诸如 unwrappedView 或 actualLabel 好。

**首选：**

```swift
var subview: UIView?
var volume: Double?
if let subview = subview, volume = volume {
	//TODO: xxx
}
```

**不推荐：**

```swift
var optionalSubview: UIView?
var volume: Double?
if let unwrappedSubview = optionalSubview {
	if let realVolume = volume {
		// ooxx
	}
}
```


## 结构体构造器

使用 Swift 原生的结构体构造器而不是传统的的 CGGeometry 构造器。

**首选：**

```swift
let bounds = CGRect(x: 40.0, y: 20.0, width: 120.0, height: 80.0)
let centerPoint = CGPoint(x: 96.0, y: 42.0)
```

**不推荐：**

```swift
let bounds = CGRectMake(40.0, 20.0, 120.0, 80.0)
let centerPoint = CGPointMake(96.0, 42.0)
```

推荐使用结构体内部的常量 CGRect.infiniteRect, CGRect.nullRect 等，来替代全局常量CGRectInfinite, CGRectNull 等。对于已经存在的变量值，可以简写成 .zero。


## 延迟初始化

延迟初始化用来细致地控制对象的生命周期，这对于想实现延迟加载视图的 UIViewController 特别有用，你可以使用即时被调用闭包或私有构造方法：

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
	let manager = CLLocationManager()
	manager.desiredAccuracy = kCLLocationAccuracyBest
	manager.delegate = self
	manager.requestAlwaysAuthorization()
	return manager
}
```

**注意：**LocationManager 的负面效果会弹出对话框要求用户提供权限，这是做延时加载的原因。


## 类型推断

推荐使用更加紧凑的代码，让编译器能够推断出每个常量和变量的类型。类型推断也适用于小的数组和字典，如果需要可以指定特定的类型，如 CGFloat 或 Int16。
	
**首选：**

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic","Sam","Christine"]
let maximumWidth: CGFloat = 106.5
```

**不推荐：**

```swift
let message:String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

## 类型注解对空的数组和字典

对空的数据和字典，使用类型注解。

**首选：**

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**不推荐：**

```swift
var names = [String]()
var lookup = [String: Int]()
```

**注意：**这意味着选择描述性名称更加重要。

## 语法糖

使用简洁的类型定义语法，而不是全称语法。

**首选：**

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**不推荐：**

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## 函数 VS 方法

自由函数不依附于任何类或类型，应该节制地使用。如果可能优先使用方法而不是自由函数，这有助于代码的可读性和易发现性。



**首选：**

```swift
let sorted = items.mergeSort() //易发现性
rocket.launch()  //可读性
```

**不推荐：**

```swift
let sorted = mergeSort(items)// 不易被发现
launch(&rocket)
```

## 自由函数

自由函数使用的场景是当不确定它和具体的类别或实例相关。

```swift
let tuples = zip(a, b)
let value = max(x,y,z) //天然自由函数
```


## 内存管理

代码应避免指针循环引用，分析对象图谱，使用弱引用和未知引用避免强引用循环。另外使用值类型（结构体和枚举）可以避免循环引用。


## 延长对象生命周期

延长对象生命周期习惯上使用 `[weak self]` 和 `guard let strongSelf = self else { return }`。

`[weak self]` 优于 `[unowned self]`，因为前者更更能明显地 self 生命周期长于闭包块，显式延长生命周期优先于可选性拆包。

**首选：**

```swift
resource.request().onComplete { [weak self] response in
	guard let strongSelf = self else {return}
	let model = strongSelf.updateModel(response)
	strongSelf.updateUI(model)
}
```

**不推荐：**

```swift
// 如果self在response之前销毁会崩溃
resource.request().onComplete { [unowned self]  response in
	let model = self.updateModel(response)
	self.updateUI(model)
}

//self可能在updateModel和updateUI被释放
resource.request().onComplete { [weak self] response in
	let model = self?.updateModel(response)
	self?.updateUI(model)
}
```


## 访问控制

合理的使用 private 和  fileprivate，推荐使用 private, 在使用 extension 时可使用 fileprivate。

访问控制符一般放在属性修饰符的最前面，除非需要使用 static 修饰符，@IBAction, @IBOutlet 或 @discardableResult。

**首选：**

```swift
class TimeMachine {
	private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
private let message = "Great Scott!"
```

**不推荐：**

```swift
class TimeMachine {
	lazy dynamic private var fluxCapacitor=FluxCapacitor()
}
fileprivate let message = "Great Scott!"
```

## 控制流程

循环使用 for-in 表达式，而不使用 while 表达式。

**首选：**

```swift
for _ in 0 ..< 3 {
	print("Hello three times")
}

for(index, person) in attendeeList.enumerate() {
	print("\(person)is at position #\(index)")
}

for index in 0.stride(from: 0, to: items.count, by: 2) {
	print(index)
}

for index in (0...3).reverse() {
	print(index)
}
```

**不推荐：**

```swift
var i=0
while i<3 {
	print("Hello three times")
	i+=1
}

var i=0
while i<3 {
	let person = attendeeList[i]
	print("\(person)is at position #\(i)")
	i+=1
}
```

## 黄金路径

当编码遇到条件判断时，左边的距离是黄金路径或幸福路径，因为路径越短，速度越快。不要嵌套 if 循环，多个返回语句是可以的。guard 就为此而生的。

**首选：**

```swift
func computeFFT(context: Context?, inputData: InputData?)  throws -> Frequencies {
	guard let context = context else {
		throwFFTError.NoContext
	}
	guard let inputData = inputData else {
		throwFFTError.NoInputData
	}
	//计算frequencies
	return frequencies
}
```

**不推荐：**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
	if let context = context {
		if let inputData = inputData {
			// 计算frequencies
			return frequencies
		} else {
			throwFFTError.NoInputData
		}
	} else {
		throwFFTError.NoContext
	}
}
```

当有多个条件需要用 guard 或 if let 解包，可用复合语句避免嵌套。

**首选：**

```swift
guard let number1 = number1, number2 = number2, number3 = number3 else {
	fatalError("impossible")
}
//TODO: 处理number
```

**不推荐：**

```swift
if let number1 = number1 {
	if let number2 = number2 {
		if let number3 = number3 {
			//TODO: 处理number
		} else {
			fatalError("impossible")
		}
	} else {
		fatalError("impossible")
	}
} else {
	fatalError("impossible")
}
```


## 失败防护

防护语句的退出有很多方式，一般都是单行语句，如 return, throw, break, continue 和fatalError() 等。 避免出现大的代码块，如果清理代码需要多个退出点，可以用 defer 模块避免重复清理代码。


## 分号

Swift 不强制每条语句有分号，当一行内写多个语句的时候是必须的。不要在一行里写多个语句。

在构造 for 递增循环时需要使用分号，但构造 for 循环时推荐使用 for-in 结构。

**首选：**

```swift
let swift = "not a scripting language"
```

**不推荐：**

```swift
let swift = "not a scripting language";
```


## 圆括号

条件判断时圆括号不是必须的，建议省略。

**首选：**

```swift
if name == "Hello" {
	print("World")
}
```

**不推荐：**

```swift
if(name == "Hello") {
	print("World")
}
```

在更复杂的表达式中，有时可选括号可以使代码更清晰地读取。

```swift
let playerMark = (player == current ? "X" : "O")
```

## 笑脸

笑脸是 raywenderlich.com 网站非常突出的风格功能，正确的笑容表示编码主题的巨大幸福感和兴奋度是非常重要的。 使用闭合方括号], 因为它代表了使用 ASCII 艺术捕获的最大笑容。 右括号）创造了一个半心半意的微笑，因此不是首选。

**首选：**

```
:]
```

**不推荐：**

```
:)
```


