# Objective-C 编码规范


## 说明

本编码规范基于 [The Official raywenderlich.com Objective-C Style Guide](https://github.com/raywenderlich/objective-c-style-guide)。

编码规范的目标是保证代码的简洁性，可读性和可维护性。

严格的编码规范，可以改善代码的的可读性，让团队中的开发人员能轻松的阅读、理解彼此的代码，从而最大限度的提高团队开发的合作效率，尽可能的减少项目的维护成本；长期的规范性编码还可以让开发人员养成好的编码习惯，甚至锻炼出更加严谨的思维。

相关：[Swift 编码规范](https://github.com/XiaXianBing/Code-Style-Guide/blob/master/Swift%20%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83.md#)


## 目录

* [语言](#语言)
* [代码组织](#代码组织)
* [间距](#间距)
* [注释](#注释)
* [命名](#命名)
  * [下划线](#下划线)
* [方法](#方法)
* [变量](#变量)
* [属性特性](#属性特性)
* [点符号语法](#点符号语法)
* [字面值](#字面值)
* [常量](#常量)
* [枚举类型](#枚举类型)
* [Case 语句](#case-语句)
* [私有属性](#私有属性)
* [布尔值](#布尔值)
* [条件语句](#条件语句)
  * [三元操作符](#三元操作符)
* [Init 方法](#init-方法)
* [类构造方法](#类构造方法)
* [CGRect 函数](#cgrect-函数)
* [黄金路径](#黄金路径)
* [错误处理](#错误处理)
* [单例模式](#单例模式)
* [换行](#换行)
* [笑脸](#笑脸)
* [Xcode 项目](#xcode-项目)
* [其他 Objective-C 编码规范](#其他-objective-c-编码规范)


## 语言

应该使用美式英语。

**首选：**

```objc
UIColor *myColor = [UIColor whiteColor];
```

**不推荐：**

```objc
UIColor *myColour = [UIColor whiteColor];
```


## 代码组织

在功能分组和 protocol/delegate 的实现中使用 `#pragma mark -` 来进行分类。

```objc
#pragma mark -
#pragma mark Lifecycle

#pragma mark -
#pragma mark Public

#pragma mark -
#pragma mark Private

#pragma mark -
#pragma mark UITextFieldDelegate

#pragma mark -
#pragma mark UITableViewDataSource && UITableViewDelegate
```


## 间距

* 使用 4 个空格缩进，可在 Xcode 中设置此首选项。
* 方法大括号和其他大括号（`if`/`else`/`switch`/`while`等）总是在同一行语句打开但在新行中关闭。

**首选：**

```objc
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```

**不推荐：**

```objc
if (user.isHappy)
{
    //Do something
}
else {
    //Do something else
}
```

* 在方法之间应该有且只有空一行，这样有利于在视觉上更清晰和更易于组织。在方法内的空行应该用作分离功能，但通常都可以抽离出来成为一个新方法。
* 优先使用 auto-synthesis。但如果有必要，`@synthesize` 和 `@dynamic` 应该在实现中每个都声明在新的一行里。
* 应该避免以冒号对齐的方式来调用方法。有时方法可能有 3 个以上的冒号，冒号对齐会使代码更加易读。但请不要这样做，如果冒号对齐的方法包含代码块，Xcode 的对齐方式将令它难以辨认。

**首选：**

```objc
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**不推荐：**

```objc
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```


## 注释

当需要注释时，注释应该用来解释这段特殊代码为什么要这样做。所有的注释都必须保持最新否则就该被删除。

通常都应该避免使用块注释，代码需要尽可能的做到自解释，只有当断断续续或几行代码时才需要注释（生成文档的注释除外）。


## 命名

应该尽可能遵守 Apple 命名约定，特别是与内存管理规则（NARC）有关的命名约定。

长的、描述性的方法和变量命名是很有必要的。

**首选：**

```objc
UIButton *settingsButton;
```

**不推荐：**

```objc
UIButton *setBut;
```

三个字符前缀应该经常用在类和常量命名，但在 CoreData 的实体名中应被省略。例如：raywenderlich.com 官方的书、初学者工具包或教程中，前缀```RWT```的使用。

常量应该使用驼峰式命名规则，所有的单词首字母大写并加上与类名有关的前缀。

**首选：**

```objc
static NSTimeInterval const RWTTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**不推荐：**

```objc
static NSTimeInterval const fadetime = 1.7;
```

属性应该使用小驼峰式（开头单词的首字母要小写）。对属性使用 auto-synthesis，而不是手动编写 `@synthesize` 语句，除非你有很好的理由。

**首选：**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**不推荐：**

```objc
id varnm;
```


### 下划线

当使用属性时，应始终使用 `self.` 来访问和改变实例变量。这就意味着所有属性将会视觉效果不同，因为它们前面都有 `self.`。

但有一个特例：在初始化方法里，应该直接使用实例变量（例如 _variableName）来避免 `getters/setters` 任何潜在的副作用。

局部变量不应该包含下划线。


## 方法

在方法签名中，应该在方法类型（-/+ 符号）后面应该有一个空格。在方法片段之间应该也有一个空格（符合 Apple 的约定）。在参数之前应该包含一个具有描述性的关键字来描述参数。

应该特别谨慎 `and` 这个词的使用。它不应该用于多个参数的说明，就像以下 `initWithWidth:height` 这个例子：

**首选：**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**不推荐：**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```


## 变量

变量应尽可能以描述性的方式来命名。应尽量避免单个字符的变量命名，除了 `for()` 循环里。

指示指针的星号属于变量，例如 `NSString *text` 既不是 `NSString* text` 也不是 `NSString * text`，除了常量的情况。

应尽可能使用私有属性代替实例变量。虽然使用实例变量是一种有效的方式，但更偏向于使用属性来保持代码一致性。

实例变量的直接访问是“返回”属性应避免除在初始化方法（init，initWithCoder:等...），dealloc方法和自定义的getter和setter方法中。有关在Initializer方法和dealloc中使用Accessor方法的更多信息，请参阅此处。

* 应该尽量避免通过使用 `back` 属性（_variable，变量名前面有下划线）直接访问实例变量，除了在初始化方法（init，initWithCoder: 等）、dealloc 方法和自定义的 setter 和 getter 方法中。有关在 Initializer 方法和 dealloc 中使用 Accessor 方法的更多信息，请参阅[此处](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

**首选：**

```objc
@interface RWTTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**不推荐：**

```objc
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```


## 属性特性

所有属性特性应该显式地列出来。属性特性的顺序应该是：原子性（nonatomic、atomic），读写权限 （readonly、readwrite），内存管理（retain、copy、assign、strong、weak、unsafe_unretained），存取方法（getter=getterName、setter=setterName）。

**首选：**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**不推荐：**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

可变类型的属性 （例如 NSString ）应该使用 copy 而不是 strong。因为即使声明一个 NSString 属性，也可能在你没有注意时通过一个 `NSMutableString` 实例赋值而改变它。  

**首选：**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**不推荐：**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```


## 点符号语法

点语法是一种很方便封装访问方法调用的方式。当你使用点语法时，该属性仍然使用getter和setter方法访问或更改。在这里阅读[更多](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)。

点语法应该**总是**被用来访问和修改属性，因为它使代码更加简洁。`[]`符号更偏向于用在其他例子。

**首选：**

```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**不推荐：**

```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```


## 字面值

`NSString`，`NSDictionary`，`NSArray` 和 `NSNumber` 的字面值应该在创建这些类的不可变实例时被使用。请特别注意 `nil` 不能传入 NSArray 和 NSDictionary 字面值，这将导致 crash。

**首选：**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**不推荐：**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```


## 常量

常量是容易重复被使用和无需通过查找和代替就能快速修改值。常量应该使用 `static` 来声明而不是使用 `#define`，除非明确地用作宏。

**首选：**

```objc
static NSString * const RWTAboutViewControllerCompanyName = @"RayWenderlich.com";

static CGFloat const RWTImageThumbnailHeight = 50.0;
```

**不推荐：**

```objc
#define CompanyName @"RayWenderlich.com"

#define thumbnailHeight 2
```


## 枚举类型

在使用 `enum` 时，推荐使用新的固定的基本类型规格，因为它有更强的类型检查和代码补全。现在 SDK 有一个宏 `NS_ENUM()` 来帮助你使用固定的基本类型。例如：

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};
```

您还可以进行明确的值分配（显示旧的 k 式常量定义）：

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```

除非编写 CoreFoundation C 代码（不太可能），否则应避免使用较旧的 k 式常量定义。

**不推荐：**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


## Case 语句

大括号在 case 语句中并不是必须的，除非编译器强制要求。当一个 case 语句包含多行代码时，大括号应该加上。

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default: 
    // ...
    break;
}
```

有时候当相同代码被多个 case 使用时，应该使用 `A fall-through`。`A fall-through` 就是在 case 最后移除 break 语句，从而允许执行流程传递给下一个 case 值。为了代码更加清晰，`A fall-through` 需要注释一下。

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}
```

当在 switch 使用枚举类型时，default 是不需要的。例如：

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```


## 私有属性

私有属性应该在类的实现文件中的类扩展（匿名分类）中声明，除非扩展另一个类，否则不应使用命名类（如 `RWTPrivate` 或 `private`）。匿名类别可以使用 `<headerfile>+Private.h` 文件的命名约定进行共享/公开测试。例如：

```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```


## 布尔值

Objective-C 使用 `YES` 和 `NO`。`true` 和 `false` 应该仅用于 CoreFoundation，C 或 C++ 代码。既然 nil 解析成 `NO`，所以没有必要在条件语句比较。不要拿某样东西直接与 `YES` 比较，因为 `YES` 被定义为 1 而 BOOL 能被设置为 8 位。

这使得跨文件更加一致，并且在视觉上更加简洁。

**首选：**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**不推荐：**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

如果一个 `BOOL` 属性的名字是形容词，属性就可以忽略 "is" 前缀，但要指定 get 访问器的惯用名称。例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

文字和示例来自于 [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)。


## 条件语句

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

条件语句主体为了防止出错应该使用大括号包围，即使条件语句主体能够不用大括号编写（如只有一行代码）。这些错误包括添加第二行代码并且期望它成为 if 语句；另一个更危险的缺陷在于，如果 if 语句里面一行代码被注释了，然后下一行代码不知不觉地成为 if 语句的一部分。此外，这种风格与其他条件语句的风格保持一致，所以更加容易阅读。

**首选：**

```objc
if (!error) {
  return success;
}
```

**不推荐：**

```objc
if (!error)
  return success;
```

或者

```objc
if (!error) return success;
```


### 三元操作符

当需要提高代码的清晰性和简洁性时，才会使用三元操作符 ` ? : `。单个条件求值常常需要它，多个条件求值时，使用 if 语句或重构成实例变量代码会更加易读。一般来说，最好使用三元操作符是在根据条件来赋值的情况下。

将非布尔变量与某些值比较时，加上括号 `()` 以提高可读性。如果被比较的变量是布尔类型，则不需要括号。

**首选：**

```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**不推荐：**

```objc
result = a > b ? x = c > d ? c : d : y;
```


## Init 方法

Init 方法应该遵循 Apple 生成代码模板的命名约定。返回类型应该使用 `instancetype` 而不是 `id`。

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

查看 [类构造方法](#类构造方法) 了解更多关于 `instancetype`。


## 类构造方法

当使用类构造方法时，应始终返回 “instancetype” 而不是 “id”。这样可以确保编译器正确地推断出结果类型。

```objc
@interface Airplane

+ (instancetype)airplaneWithType:(RWTAirplaneType)type;

@end
```
关于 `instancetype` 的更多信息可以在 [NSHipster.com](http://nshipster.com/instancetype/) 上找到。


## CGRect 函数

当访问 CGRect 里的 x、y、width、height 时，应该使用 CGGeometry 函数而不是直接通过结构体成员来访问。引用 Apple 的 CGGeometry：

> 在这个参考文档中所有的函数，接受 CGRect 结构体作为输入，在计算它们的结果时隐式地标准化这些矩形。因此，你的应用程序应该避免直接访问和修改 CGRect 数据结构中的数据。相反，使用这些函数来操纵这些矩形和获取它们的特性。

**首选：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**不推荐：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```


## 黄金路径

当使用条件语句编码时，左边的代码应该是 “golden” 或 “happy” 路径。也就是说，不要嵌套 `if` 语句，多个返回语句可以的。

**首选：**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**不推荐：**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```


## 错误处理

当方法通过引用来返回一个错误参数，判断返回值而不是错误变量。

**首选：**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**不推荐：**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

在成功的情况下， Apple 的一些 API 将错误参数（如果为非空）写入垃圾值，那么判断错误值会导致 false 负值，然后崩溃。


## 单例模式

单例对象应该使用线程安全模式来创建共享实例。

```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```


## 换行

换行是一个重要的话题，因为这种编码规范专注于打印和在线可读性。例如：

```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```
这样长的代码行应该遵循这种风格指南的间距部分（两个空格）。

```objc
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
```


## 笑脸

笑脸是 raywenderlich.com 网站非常突出的风格！拥有正确的笑容是相当重要的，它表达了编码主题的巨大幸福感和兴奋度。使用最终的方括号是因为它代表了使用 ASCII 编码艺术捕获的最大的笑容。如果使用后括号，则表示半心半意的笑容，因此不是首选。

**首选：**

```objc
:]
```

**不推荐：**

```objc
:)
```  


## Xcode 项目

物理文件应与 Xcode 项目文件保持同步以避免文件扩展。任何 Xcode 分组的创建都应该在文件系统中体现。代码不仅是根据类型来分组，而且还可以根据功能来分组，这样代码更加清晰。

可能的话，打开 `Build Settings` 下 `Treat Warnings as Errors`，把警告当做错误处理，这条规则从根本禁止了一些文法使用。并尽可能多的启用其他警告：[additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)，如果你需要忽略特定的警告，请使用 [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) 。


# 其他 Objective-C 编码规范

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

