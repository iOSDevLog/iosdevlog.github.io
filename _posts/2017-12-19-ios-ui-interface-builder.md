---
layout: post
title: "《iOS UI 开发捷径 利用 Interface Builder 高效、优雅地开发 UI》 读书笔记"
author: iosdevlog
date: 2017-12-19 08:24:53 +0800
description: ""
category: iOS
tags: [UI, 读书笔记]
---

# 第1章 Interface Bundle 概要
---

## Bundle

一种标准化的层次结构，保存了可执行代码及代码所需要的资源。

## nib

Next Interface Builder

## Interface Builder 的优点

1. 开发和维护效率高
1. 减少大量的 UI 代码和“胶水代码”
1. 适配变得十分简单
1. IB 也可以做一些非 UI 的事情
1. 利用 IB 学习控件可以达到事半功倍的效果

## Interface Builder 的缺点

1. IB 的执行效率没有纯代码高
1. 使用 IB 开发的过程中容易出现一些小问题
1. 有一定的学习成本
1. 文件易冲突
1. 没有代码表达清晰
1. 不利于代码的封闭和工程架构的组织

## Interface Builder 学习的特点

1. 简单，容易入门
1. 容易犯错误
1. 有很多的“坑”，需要积累属于自己的经验

## Interface Builder 的发展

xib -> sb -> AutoLayout -> LaunchScreen.storyboard

苹果越来越重视 IB。

## CocoaPods

```ruby
# 1 最新版本
pod 'AFNetworking'

# 2 最新的2.x版本
pod 'AFNetworking', '~>2.5.3'

# 3 指定版本
pod 'AFNetworking', '2.5.3' 
```

试用

```
$ pod try AFNetworking
```

## Podfile文件与CocoaPods的三种依赖方式

* 远程依赖

```ruby
# master
pod 'Alamofile', :git => 'https://github.com/Alamofile/Alamofile.git'

# branch
pod 'Alamofile', :git => 'https://github.com/Alamofile/Alamofile.git', :branch -> 'dev'

# commit
pod 'Alamofile', :git => 'https://github.com/Alamofile/Alamofile.git', :commit => '0f506b1c45'
```

* 本地依赖

```
pod 'AFNetworking', :path => '../externals/libs/AFNetworking'
```

* 私有依赖

```
pod 'AFNetworking', :podspec => '../externals/libs/AFNetworking.podspec'
```

> “：podspec=>”用于指定本地的一个xxx.podspec文件。

## podspec文件

```
$ pod init
$ cat Podfile
```

# 第2章 使用 Interface Builder
---

## 解决冲突

1. 解决普通IB文件冲突

Open As -> Source Code

```
<<<<<<<

=======

>>>>>>>
```

编辑好，再删除这三行就可以了。

2. 解决 Xcode 8 引起的 IB 文件冲突

如果用 Source Code 不能打开，就用文件编辑器(vim, etc)打开，把*systemVersion*等冲突解决。

### 关联 xib 文件与源文件

1. 关联 xib 文件与 UIView 子类的源文件

* 新建一个空的*xib*文件，拖一个*UIView*上去
* 新建一个继承自*UIView*的源文件
* 选中*xib*文件里的*View*,把**class**改为上面

### @IBOutlet 与 @IBAction

连线

## 设计模式之*MVC*

![mvc](/assets/images/book/IB/mvc.png)

## 理解 File's Owner

## 关联 xib 文件与 UIViewController 子类的源文件

* 自定义一个 VC 的 View 的两种方法

1. 在 IB 文件中选中 VC 所在的 View，在 Show the Identity inspector 中设置 Class 标签的值为自定义的 View 的类名。
1. 在源文件的 *loadView()* 方法里设置该 VC 的 View 属性为这个自定义的 View。

```swift
override func loadView() {
    self.view = CustomView.init(frame: UIScreen.main.bounds)
}
```

* File's Owner 指向 VC

* VC 的 View 连线

> xib 既可以与 UIView 关联，也可以与 UIViewController 关联，也可以同时关联 UIView 与 UIViewController

## 使用 xib

1. 通过 Bundle 方式加载
1. 通过 UINib 方式加载

### 使用与UIView子类源文件关联的xib

#### Bundle

```
- (NSArray *)loadNibNamed:(NSString *)name 
                    owner:(id)owner 
                  options:(NSDictionary *)options;
```

```swift
func loadNibNamed(_ name: String, 
            owner: Any?, 
          options: [AnyHashable : Any]? = nil) -> [Any]?
```

Parameters 参数

* name
The name of the nib file, which need not include the .nib extension.
nib名称
* owner
The object to assign as the nib’s File's Owner object.
如果xib文件有File's Owner，一定传其实例对象，否则传nil
* options
A dictionary containing the options to use when opening the nib file. For a list of available keys for this dictionary, see UIKit Nib Loading Options.

##### Bundle Demo

```
let testView = Bundle.main.loadNibNamed("TestView", owner: nil, options: nil)?[0] as! UIView
view.addSubview(testView)
```

#### UINib

```
NS_CLASS_AVAILABLE_IOS(4_0) @interface UINib : NSObject 

// If the bundle parameter is nil, the main bundle is used.
// Releases resources in response to memory pressure (e.g. memory warning), reloading from the bundle when necessary.
+ (UINib *)nibWithNibName:(NSString *)name bundle:(nullable NSBundle *)bundleOrNil;

// If the bundle parameter is nil, the main bundle is used.
+ (UINib *)nibWithData:(NSData *)data bundle:(nullable NSBundle *)bundleOrNil;

// Returns an array containing the top-level objects from the NIB.
// The owner and options parameters may both be nil.
// If the owner parameter is nil, connections to File's Owner are not permitted.
// Options are identical to the options specified with -[NSBundle loadNibNamed:owner:options:]
- (NSArray *)instantiateWithOwner:(nullable id)ownerOrNil options:(nullable NSDictionary *)optionsOrNil;
@end
```

```swift
@available(iOS 4.0, *)
open class UINib : NSObject {

    
    // If the bundle parameter is nil, the main bundle is used.
    // Releases resources in response to memory pressure (e.g. memory warning), reloading from the bundle when necessary.
    public /*not inherited*/ init(nibName name: String, bundle bundleOrNil: Bundle?)

    
    // If the bundle parameter is nil, the main bundle is used.
    public /*not inherited*/ init(data: Data, bundle bundleOrNil: Bundle?)

    
    // Returns an array containing the top-level objects from the NIB.
    // The owner and options parameters may both be nil.
    // If the owner parameter is nil, connections to File's Owner are not permitted.
    // Options are identical to the options specified with -[NSBundle loadNibNamed:owner:options:]
    open func instantiate(withOwner ownerOrNil: Any?, options optionsOrNil: [AnyHashable : Any]? = nil) -> [Any]
}
```

##### UINib Demo

```swift
let testViewNib = UINib.init(nibName: "TestView", bundle: Bundle.main)
```

```swift
func loadTestView() {
    let testView = UINib.instantiate(withOwner: nil, options: nil)[0] as! UIView
    view.addSubview(testView)
}
```

#### 使用与 UIViewController 子类源文件关联的 xib

###### Demo HomeViewController.swift <==> HomeController.xib

```swift
let homeVC = HomeViewController()
```

```swift
class HomeViewController {
    override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: Bundle?) {
        super.init(nibName: "HomeController", bundle: nil)
    }

    init(aName: String) {
        name = aName
        super.init(nibName: "HomeController", bundle: nil)
    }

    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
}
```

![1](https://github.com/iOSDevLog/iOSDevLog/raw/master/228.%20NestedXib/1.png)

![2](https://github.com/iOSDevLog/iOSDevLog/raw/master/228.%20NestedXib/2.png)

![3](https://github.com/iOSDevLog/iOSDevLog/raw/master/228.%20NestedXib/3.png)

![4](https://github.com/iOSDevLog/iOSDevLog/raw/master/228.%20NestedXib/4.png)

## 使用 storyboard

```swift
@available(iOS 5.0, *)
open class UIStoryboard : NSObject {

    
    public /*not inherited*/ init(name: String, bundle storyboardBundleOrNil: Bundle?)

    
    open func instantiateInitialViewController() -> UIViewController?

    open func instantiateViewController(withIdentifier identifier: String) -> UIViewController
}
```

storyboard Demo

```swift
extension UIViewController {
    class func storyboardID() -> String {
        return String(describing: self)
    }
}

let userStoryBoard = UIStoryboard(name: "User", bundle: nil)
let profileVC = userStoryBoard.instantiateInitialViewController(withIdentifier: ProfileController.storyboardID())
```

# 第3章 全面学习 xib
---

## Autoresizing

![Autoresizing](/assets/images/book/IB/Autoresizing.png)

默认选中左，上。

对应代码为：

```swift
testView.autoresizingMask = [.flexibleRightMargin, .flexibleBottomMargin]
```

外框的 上、下、左、右如果选中，则UIView的边框与父View边框距离保持不变。

中间带箭头的选中表示UIView边框是随屏幕尺寸变化的。
否则UIView大小保持不变。

也可以看右侧的动画查看显示效果。

# 第4章 在 Interface Builder 中使用 Auto Layout
---

### 在 IB 中使用 Auto Layout 的优缺点

* 设置约束十分简单
* 如果约束不恰当，IB 提供很好的实时反馈
* 如果约束不恰当，IB 可以帮忙改正。改正操作十分简单、方便、快捷。

* 难以理解

## 约束

### Auto Layout 的数学公式

```
item1.attribute1 = multiplier * item2.attribute2 + constant
```

### 约束属性

```swift
public enum NSLayoutAttribute : Int {
    case left
    case right
    case top
    case bottom
    case leading
    case trailing
    case width
    case height
    case centerX
    case centerY
    case lastBaseline
    @available(iOS 8.0, *)
    case firstBaseline
    @available(iOS 8.0, *)
    case leftMargin
    @available(iOS 8.0, *)
    case rightMargin
    @available(iOS 8.0, *)
    case topMargin
    @available(iOS 8.0, *)
    case bottomMargin
    @available(iOS 8.0, *)
    case leadingMargin
    @available(iOS 8.0, *)
    case trailingMargin
    @available(iOS 8.0, *)
    case centerXWithinMargins
    @available(iOS 8.0, *)
    case centerYWithinMargins
    case notAnAttribute
}
```

### 约束关系

```swift
public enum NSLayoutRelation : Int {
    case lessThanOrEqual
    case equal
    case greaterThanOrEqual
}
```

### 约束的优先级

```swift
public struct UILayoutPriority : RawRepresentable, Equatable, Hashable {
    public init(_ rawValue: Float)
    public init(rawValue: Float)
}

extension UILayoutPriority {
    @available(iOS 6.0, *)
    public static let required: UILayoutPriority

    @available(iOS 6.0, *)
    public static let defaultHigh: UILayoutPriority // This is the priority level with which a button resists compressing its content.

    @available(iOS 6.0, *)
    public static let defaultLow: UILayoutPriority // This is the priority level at which a button hugs its contents horizontally.

    @available(iOS 6.0, *)
    public static let fittingSizeLevel: UILayoutPriority // When you send -[UIView systemLayoutSizeFittingSize:], the size fitting most closely to the target size (the argument) is computed.  UILayoutPriorityFittingSizeLevel is the priority level with which the view wants to conform to the target size in that computation.  It's quite low.  It is generally not appropriate to make a constraint at exactly this priority.  You want to be higher or
    lower.
}
```

### Instrinsic Size 固有尺寸

默认设置了 Width 和 Height

### Content Compression Resistance 压缩阻力

### Content Hugging 内容吸附

### NSLayoutConstraint 与 @IBOutlet 连线

## 设置约束的方法

1. 在 IB 中设置 - 推荐
1. 苹果原生 API - 最复杂，强烈不推荐
1. 用 VFL (Visual Format Language) 设置约束 - 不推荐
1. 第三方库（Masonry等）设置约束 - 代码设置最简单、最常用 推荐(如果不熟悉 Auto Layout)

## UIStackView

Axis

* Vertical: 竖直布局
* Horizontal: 水平布局

Alignment

* Fill
* Top
* Leading
* Center
* Bottom
* Trailing
* First Baseline
* Last Base Line

Distribution

* Fill
* Fill Equal
* Fill Proportionally
* Equal Spacing
* Equal Centering

Space

Baseline Relative

### FDStackView

### NSUUID <https://gist.github.com/OliverLetterer/4643294>

## Auto Layout 的异类 - UIScrollView

UIScrollView 的子 View 需要设置 6 个约束

```
scrollView.contentSize.width = subView.leading + subView.width + subView.trailing;
scrollView.contentSize.height = subView.top + subView.height+ subView.bottom;
```

> 设置 ScrollView 的子 View 约束时一定要让系统确定 ScrollView 的 contentSize。

# 第5章 storyboard 全面学习
---

## Extra View

## segue

### Embed Segue

### Unwind Segue

## Launch Screen

```
@interface AppDelegate ()
            
@property (nonatomic, strong) UIWindow* launchWindow;

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.launchWindow = [[UIWindow alloc] initWithFrame:UIScreen.mainScreen.bounds];
    self.launchWindow.hidden = NO;
    self.launchWindow.windowLevel = UIWindowLevelNormal + 1;
    self.launchWindow.rootViewController = [UIStoryboard storyboardWithName:@"Launch Screen" bundle:nil].instantiateInitialViewController;
    [UIView animateWithDuration:3 delay:0.5 options: UIViewAnimationOptionCurveEaseInOut animations:^{
        self.launchWindow.alpha = 0;
    } completion:^(BOOL finished) {
        self.launchWindow.hidden = YES;
        self.launchWindow.windowLevel = UIWindowLevelNormal - 1;
        self.launchWindow.alpha = 1;
    }];
    
    return YES;
}
@end
```

```swift
var launchWindow: UIWindow?

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    launchWindow = UIWindow(frame: UIScreen.main.bounds)
    launchWindow?.windowLevel = UIWindowLevelNormal + 1
    launchWindow?.isHidden = false
    launchWindow?.rootViewController = UIStoryboard(name: "LaunchScreen", bundle: nil).instantiateInitialViewController()
    UIView.animate(withDuration: 0.5, delay: 3, options: .curveEaseInOut, animations: {
        self.launchWindow?.alpha = 0
    }) { (finish) in
        if finish {
            self.launchWindow?.isHidden = true
            self.launchWindow?.windowLevel = UIWindowLevelNormal - 1
            self.launchWindow?.alpha = 1
        }
    }
    return true
}
```
