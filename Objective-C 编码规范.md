# Objective-C 编码规范

编码规范的目标是保证代码的简洁性，可读性和可维护性。

严格的编码规范，可以改善代码的的可读性，让团队中的开发人员能轻松的阅读、理解彼此的代码，从而最大限度的提高团队开发的合作效率，尽可能的减少项目的维护成本；长期的规范性编码还可以让开发人员养成好的编码习惯，甚至锻炼出更加严谨的思维。


## 说明

本编码规范基于 [The Official raywenderlich.com Objective-C Style Guide](https://github.com/raywenderlich/objective-c-style-guide)。

关于这个编程语言的所有规范，如果这里没有写到，那就在苹果的文档里： 

* [Objective-C 编程语言](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa 基本原理指南](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Cocoa 编码指南](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS 应用编程指南](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)


## 目录

* [语言](#语言)
* [Xcode 项目](#xcode-项目)
* [代码组织](#代码组织)
* [导入](#导入)
* [间距](#间距)
* [注释](#注释)
* [命名](#命名)
  * [下划线](#下划线)
* [方法](#方法)
* [变量](#变量)
* [属性特性](#属性特性)
* [点语法](#点语法)
* [字面量](#字面量)
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


## Xcode 项目

物理文件应与 Xcode 项目文件保持同步以避免文件扩展。任何 Xcode 分组的创建都应该在文件系统中体现。代码不仅是根据类型来分组，而且还可以根据功能来分组，这样代码更加清晰。

可能的话，打开 `Build Settings` 下 `Treat Warnings as Errors`，把警告当做错误处理，这条规则从根本禁止了一些文法使用。并尽可能多的启用其他警告：[Additional Warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)，如果你需要忽略特定的警告，请使用 [Clang 的编译特性](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) 。


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


## 导入

如果有一个以上的 import 语句，就对这些语句进行[分组](http://ashfurrow.com/blog/structuring-modern-objective-c)。每个分组的注释是可选的。   

注：对于模块使用 [@import](http://clang.llvm.org/docs/Modules.html#using-modules) 语法。**举例：**

```objc   
// Frameworks
@import QuartzCore;

// Models
#import "RWTUser.h"

// Views
#import "RWTButton.h"
#import "RWTUserView.h"
```


## 间距

一个缩进使用 4 个空格，永远不要使用制表符（tab）缩进。请确保在 Xcode 中设置了此偏好。

方法的大括号和其他的大括号（`if`/`else`/`switch`/`while` 等等）始终和声明在同一行开始，在新的一行结束。

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

方法之间应该正好空一行，这有助于视觉清晰度和代码组织性。在方法中的功能块之间应该使用空白分开，但往往可能应该创建一个新的方法。

`@synthesize` 和 `@dynamic` 在实现中每个都应该占一个新行。

避免以冒号对齐的方式来调用方法。有时方法可能有 3 个以上的冒号，冒号对齐会使代码更加易读。但请不要这样做，如果冒号对齐的方法包含代码块，Xcode 的对齐方式将令它难以辨认。

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

当需要的时候，注释应该被用来解释 **为什么** 特定代码做了某些事情。所使用的任何注释必须保持最新否则就删除掉。

通常应该避免一大块注释，代码就应该尽量作为自身的文档，只需要隔几行写几句说明。这并不适用于那些用来生成文档的注释。


## 命名

尽可能遵守苹果的命名约定，尤其那些涉及到[内存管理规则](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)的。

长的、描述性的方法名和变量命名是很有必要的。

**首选：**

```objc
UIButton *settingsButton;
```

**不推荐：**

```objc
UIButton *setBut;
```

类名和常量应该始终使用三个字母的前缀（Apple 保留两个字母的前缀，建议用三个字母做前缀），但 CoreData 实体名称可以省略。为了代码清晰，常量应该使用相关类的名字作为前缀并使用驼峰命名法。

**首选：**

```objc
static NSTimeInterval const RWTTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**不推荐：**

```objc
static NSTimeInterval const fadetime = 1.7;
```

属性和局部变量应该使用驼峰命名法并且首字母小写（小驼峰命名法）。为了保持一致，实例变量应该使用驼峰命名法命名，并且首字母小写，以下划线为前缀。这与 LLVM 自动合成的实例变量相一致。**如果 LLVM 可以自动合成变量，那就让它自动合成。**

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

在方法签名中，在方法类型（-/+ 符号）后应该有一个空格。方法片段之间也应该有一个空格（符合 Apple 的约定）。在参数之前应该包含一个具有描述性的关键字来描述参数。

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

变量名应该尽可能命名为描述性的。除了 `for()` 循环外，其他情况都应该避免使用单字母的变量名。

星号表示指针属于变量，例如：`NSString *text` 不要写成 `NSString* text` 或者 `NSString * text` ，常量除外。

尽量定义属性来代替直接使用实例变量。除了初始化方法（`init`， `initWithCoder:`，等）， `dealloc` 方法和自定义的 setters 和 getters 内部，应避免直接访问实例变量。更多有关在初始化方法和 dealloc 方法中使用访问器方法的信息，参见[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

当涉及到[在 ARC 中被引入](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4)变量限定符时，
限定符（`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing`）应该位于星号和变量名之间，如：`NSString * __weak text`。

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


## 点语法

点语法是一种很方便封装访问方法调用的方式。当你使用点语法时，该属性仍然使用getter和setter方法访问或更改。在这里阅读[更多](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)。

应该 **始终** 使用点语法来访问和修改属性，访问其他实例时首选`[]`。

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


## 字面量

每当创建 `NSString`， `NSDictionary`， `NSArray`，和 `NSNumber` 类的不可变实例时，都应该使用字面量。要注意 `nil` 值不能传给 `NSArray` 和 `NSDictionary` 字面量，这样做会导致崩溃。

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

常量首选内联字符串字面量或数字，因为常量可以轻易重用并且可以快速改变而不需要查找和替换。常量应该声明为 `static` 常量而不是 `#define` ，除非非常明确地要当做宏来使用。

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

当使用 `enum` 时，建议使用新的基础类型规范，因为它具有更强的类型检查和代码补全功能。现在 SDK 包含了一个宏来鼓励使用使用新的基础类型：`NS_ENUM()`。

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

当用到位掩码时，使用 `NS_OPTIONS` 宏。**举例：**

```objc
typedef NS_OPTIONS(NSUInteger, RWTAdCategory) {
    RWTAdCategoryAutos      = 1 << 0,
    RWTAdCategoryJobs       = 1 << 1,
    RWTAdCategoryRealState  = 1 << 2,
    RWTAdCategoryTechnology = 1 << 3
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

私有属性应该声明在类实现文件的延展（匿名的类目）中，除非扩展另一个类，否则不应使用命名类（如 `RWTPrivate` 或 `private`）。匿名类别可以使用 `<headerfile>+Private.h` 文件的命名约定进行共享/公开测试。例如：

```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```


## 布尔值

Objective-C 使用 `YES` 和 `NO`。`true` 和 `false` 应该仅用于 CoreFoundation，C 或 C++ 代码。因为 `nil` 解析为 `NO`，所以没有必要在条件中与它进行比较。永远不要直接和 `YES` 进行比较，因为 `YES` 被定义为 1，而 `BOOL` 可以多达 8 位。这使得跨文件更加一致，并且在视觉上更加简洁。

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

如果一个 `BOOL` 属性名称是一个形容词，属性可以省略 "is" 前缀，但要为 get 访问器指定一个惯用的名字，例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```

内容和例子来自 [Cocoa 命名指南](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)。


## 条件语句

条件语句主体为了防止[出错](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256)应该使用大括号包围，即使条件语句主体能够不用大括号编写（如只有一行代码）。这些错误包括添加第二行代码并且期望它成为 if 语句；另一个[更危险的](http://programmers.stackexchange.com/a/16530)缺陷在于，如果 if 语句里面一行代码被注释了，然后下一行代码不知不觉地成为 if 语句的一部分。此外，这种风格与其他条件语句的风格保持一致，所以更加容易阅读。

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

**或者**

```objc
if (!error) return success;
```


### 三元操作符

只有当可以提高代码的清晰度和简洁性时，才会使用三元操作符 ` ? : `。单一的条件都应该优先考虑使用。多条件时通常使用 if 语句会更易懂，或者重构为实例变量。一般来说，最好使用三元操作符是在根据条件来赋值的情况下。

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

`dealloc` 方法应该放在实现文件的最上面，并且刚好在 `@synthesize` 和 `@dynamic` 语句的后面。在任何类中，`init` 都应该直接放在 `dealloc` 方法的下面。

Init 方法应该遵循 Apple 生成代码模板的命名约定。返回类型应该使用 `instancetype` 而不是 `id`。`init` 方法的结构应该像这样：

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // Custom initialization
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


当访问一个 `CGRect` 的 `x`， `y`， `width`， `height` 时，应该使用[`CGGeometry` 函数](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)代替直接访问结构体成员。引用 Apple 的 `CGGeometry` 参考：
> 在这个参考文档中所有的函数，接受 CGRect 结构体作为输入，在计算它们的结果时隐式地标准化这些矩形。因此，你的应用程序应该避免直接访问和修改 CGRect 数据结构中的数据。相反，使用这些函数来操纵这些矩形和获取它们的特性。

**推荐：**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**反对：**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

[CGRect-Functions_1]:


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

当引用一个返回错误参数的方法时，应该判断返回值而不是错误变量。

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

一些苹果的 API 在成功的情况下会写一些垃圾值给错误参数（如果非空），那么判断错误变量可能会造成虚假结果（以及接下来的崩溃）。


## 单例

单例对象应该使用线程安全的模式创建共享的实例。

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

这将会预防[有时可能产生的许多崩溃](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)。


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


# 其他 Objective-C 编码规范

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](https://github.com/google/styleguide/blob/gh-pages/objcguide.md)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)




