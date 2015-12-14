# Object-C代码规范 1.0
##命名
Apple命名规则尽可能坚持，特别是与这些相关的memory management rules (NARC)。

长的，描述性的方法和变量命名是好的。

####应该:
```objective-c
UIButton *settingsButton;
```
####不应该:
```objective-c
UIButton *setBut;
```

在命名API元素时， 使用驼峰命名法（如runTheWordsTogether），并注意以下书写约定：

###方法名
小写第一个字母，大写之后所有单词的首字母，不使用前缀

 如果方法名以一个众所周知的大写缩略词开始，该规则不适用如:
```objective-c
TIFFRepresentation (NSImage)
fileExistsAtPath:isDirectory:
```
 
###函数及常量名
使用与其关联类相同的前缀，并大写首字母
```objective-c
NSRunAlertPanel
NSCellDisabled 
```
 
###标点符号
由多个单词组成的名称，别使用标点符号作为名称的一部分

分隔符（下划线、破折号等）也不能使用

避免使用下划线作为私有方法的前缀，Apple保留这一方式的使用

强行使用可能会导致命名冲突，即Apple已有的方法被覆盖，这会导致灾难性后果

 实例变量使用下划线作为前缀还是允许的

 
###class与protocol命名

class的名称应该包含一个名词，用以表明这个类是什么（或者做了什么），并拥有合适的前缀，如NSString、NSDate、NSScanner、UIApplication、UIButton

 

不关联class的protocol
大多数protocol聚集了一堆相关方法，并不关联class
这种protocol使用ing形式以和class区分开来

##代码组织
在函数分组和protocol/delegate实现中使用#pragma mark -来分类方法，要遵循以下一般结构：
```objective-c
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```
##空格
缩进使用4个空格，确保在Xcode偏好设置来设置。(raywenderlich.com使用2个空格)
方法大括号和其他大括号(if/else/switch/while 等.)总是在同一行语句打开但在新行中关闭。
####应该:
```objective-c
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```
在方法之间应该有且只有一行，这样有利于在视觉上更清晰和更易于组织。在方法内的空白应该分离功能，但通常都抽离出来成为一个新方法。
优先使用auto-synthesis。但如果有必要，@synthesize 和 @dynamic应该在实现中每个都声明新的一行。
应该避免以冒号对齐的方式来调用方法。因为有时方法签名可能有3个以上的冒号和冒号对齐会使代码更加易读。请不要这样做，尽管冒号对齐的方法包含代码块，因为Xcode的对齐方式令它难以辨认。
####应该:
```objective-c
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```
##注释
当需要注释时，注释应该用来解释这段特殊代码为什么要这样做。任何被使用的注释都必须保持最新或被删除。

一般都避免使用块注释，因为代码尽可能做到自解释，只有当断断续续或几行代码时才需要注释。例外：这不应用在生成文档的注释
##下划线
当使用属性时，实例变量应该使用self.来访问和改变。这就意味着所有属性将会视觉效果不同，因为它们前面都有self.。

但有一个特例：在初始化方法里，实例变量(例如，_variableName)应该直接被使用来避免getters/setters潜在的副作用。

局部变量不应该包含下划线。
##方法
在方法签名中，应该在方法类型(-/+ 符号)之后有一个空格。在方法各个段之间应该也有一个空格(符合Apple的风格)。在参数之前应该包含一个具有描述性的关键字来描述参数。

"and"这个词的用法应该保留。它不应该用于多个参数来说明，就像initWithWidth:height以下这个例子：
####应该:
```objective-c
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```
##变量
变量尽量以描述性的方式来命名。单个字符的变量命名应该尽量避免，除了在for()循环。

星号表示变量是指针。例如， NSString *text 既不是 NSString* text 也不是 NSString * text，除了一些特殊情况下常量。

[私有变量](http://www.jianshu.com/p/8b76814b3663#private-properties) 应该尽可能代替实例变量的使用。尽管使用实例变量是一种有效的方式，但更偏向于使用属性来保持代码一致性。

通过使用'back'属性(_variable，变量名前面有下划线)直接访问实例变量应该尽量避免，除了在初始化方法(init, initWithCoder:, 等…)，dealloc 方法和自定义的setters和getters。想了解关于如何在初始化方法和dealloc直接使用Accessor方法的更多信息，查看[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)
####应该:
```objective-c
@interface RWTTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```
##属性特性
所有属性特性应该显式地列出来，有助于新手阅读代码。属性特性的顺序应该是storage、atomicity，与在Interface Builder连接UI元素时自动生成代码一致。

####应该:
```objective-c
@property (weak, nonatomic) IBOutlet UIView *containerView;

@property (strong, nonatomic) NSString *tutorialName;
```
####不应该:
```objective-c
@property (nonatomic, weak) IBOutlet UIView *containerView;

@property (nonatomic) NSString *tutorialName;

NSString应该使用copy 而不是 strong的属性特性。
```
为什么？即使你声明一个NSString的属性，有人可能传入一个NSMutableString的实例，然后在你没有注意的情况下修改它。
####应该:
```objective-c
@property (copy, nonatomic) NSString *tutorialName;
```
####不应该:
```objective-c
@property (strong, nonatomic) NSString *tutorialName;
```

##常量
常量是容易重复被使用和无需通过查找和代替就能快速修改值。常量应该使用static来声明而不是使用#define，除非显式地使用宏。

####应该:
```objective-c
static NSString * const RWTAboutViewControllerCompanyName = @"RayWenderlich.com";

static CGFloat const RWTImageThumbnailHeight = 50.0;
```
####不应该:
```objective-c
#define CompanyName @"RayWenderlich.com"

#define thumbnailHeight 2
 ```
##枚举类型
当使用enum时，推荐使用新的固定基本类型规格，因为它有更强的类型检查和代码补全。现在SDK有一个宏NS_ENUM()来帮助和鼓励你使用固定的基本类型。例如:
```objective-c
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
 RWTLeftMenuTopItemMain,
 RWTLeftMenuTopItemShows,
 RWTLeftMenuTopItemSchedule
};
```   
你也可以显式地赋值(展示旧的k-style常量定义)：
```objective-c
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
 RWTPinSizeMin = 1,
 RWTPinSizeMax = 5,
 RWTPinCountMin = 100,
 RWTPinCountMax = 500,
};
```    
旧的k-style常量定义应该避免除非编写Core Foundation C的代码。

####不应该:
```objective-c
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```	
##Case语句
大括号在case语句中并不是必须的，除非编译器强制要求。当一个case语句包含多行代码时，大括号应该加上。
```objective-c
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
有很多次，当相同代码被多个cases使用时，一个fall-through应该被使用。一个fall-through就是在case最后移除'break'语句，这样就能够允许执行流程跳转到下一个case值。为了代码更加清晰，一个fall-through需要注释一下。
```objective-c
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
当在switch使用枚举类型时，'default'是不需要的。例如：
```objective-c
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
##私有属性
私有属性应该在类的实现文件中的类扩展(匿名分类)中声明，命名分类(比如RWTPrivate或private)应该从不使用除非是扩展其他类。匿名分类应该通过使用<headerfile>+Private.h文件的命名规则暴露给测试。

###例如:
```objective-c
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```	
##布尔值
Objective-C使用YES和NO。因为true和false应该只在CoreFoundation，C或C++代码使用。既然nil解析成NO，所以没有必要在条件语句比较。不要拿某样东西直接与YES比较，因为YES被定义为1和一个BOOL能被设置为8位。
这是为了在不同文件保持一致性和在视觉上更加简洁而考虑。

####应该:
```objective-c
if (someObject) {}
if (![anotherObject boolValue]) {}
```	
####不应该:
```objective-c
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```	
如果BOOL属性的名字是一个形容词，属性就能忽略"is"前缀，但要指定get访问器的惯用名称。例如：
```objective-c
@property (assign, getter=isEditable) BOOL editable;
```
文字和例子从这里引用[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)

##条件语句
条件语句主体为了防止出错应该使用大括号包围，即使条件语句主体能够不用大括号编写(如，只用一行代码)。这些错误包括添加第二行代码和期望它成为if语句；还有，even more dangerous defect可能发生在if语句里面一行代码被注释了，然后下一行代码不知不觉地成为if语句的一部分。除此之外，这种风格与其他条件语句的风格保持一致，所以更加容易阅读。

####应该:
```objective-c
if (!error) {
  return success;
}
```	
####不应该:
```objective-c
if (!error)
  return success;
```	  
###或
```objective-c
if (!error) return success;
```
##三元操作符
当需要提高代码的清晰性和简洁性时，三元操作符?:才会使用。单个条件求值常常需要它。多个条件求值时，如果使用if语句或重构成实例变量时，代码会更加易读。一般来说，最好使用三元操作符是在根据条件来赋值的情况下。

Non-boolean的变量与某东西比较，加上括号()会提高可读性。如果被比较的变量是boolean类型，那么就不需要括号。

####应该:
```objective-c
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```	
####不应该:
```objective-c
result = a > b ? x = c > d ? c : d : y;
```
##Init方法
Init方法应该遵循Apple生成代码模板的命名规则。返回类型应该使用instancetype而不是id
```objective-c
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```
查看关于instancetype的文章Class Constructor Methods


##类构造方法
当类构造方法被使用时，它应该返回类型是instancetype而不是id。这样确保编译器正确地推断结果类型。
```objective-c
@interface Airplane
+ (instancetype)airplaneWithType:(RWTAirplaneType)type;
@end
```
关于更多instancetype信息，请查看[NSHipster.com](http://nshipster.com/instancetype/)


##CGRect函数
当访问CGRect里的x, y, width, 或 height时，应该使用CGGeometry函数而不是直接通过结构体来访问。引用Apple的CGGeometry:

在这个参考文档中所有的函数，接受CGRect结构体作为输入，在计算它们结果时隐式地标准化这些rectangles。因此，你的应用程序应该避免直接访问和修改保存在CGRect数据结构中的数据。相反，使用这些函数来操纵rectangles和获取它们的特性。
####应该:
```objective-c
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```
####不应该:
```objective-c
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```
##单例模式
单例对象应该使用线程安全模式来创建共享实例。

```objective-c
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

这会防止[possible and sometimes prolific crashes.](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)

-------
参考自[The official raywenderlich.com Objective-C style guide](https://github.com/raywenderlich/objective-c-style-guide#language)

Author: BillXie


