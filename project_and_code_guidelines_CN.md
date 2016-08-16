# 1. 项目指引

## 1.1 项目结构

新的项目应当遵循定义在这里的结构[Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure). 这里的 [ribot 脚手架](https://github.com/ribot/android-boilerplate) 项目是个不错的起点.

## 1.2 文件命名

### 1.2.1 类文件
类名使用 [驼峰式命名](http://en.wikipedia.org/wiki/CamelCase).

对于继承自Android自身组件的类，类名应该在末尾保留该组件名，例如： `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.


### 1.2.2 资源文件

资源文件名应该用下划线形式
`lowercase_underscore`.

#### 1.2.2.1 Drawable 文件

drawables文件的命名习惯:


| 资源类型   | 前缀            |		示例               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

图标文件命名习惯 (引用自 [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| 资源类型                      | 前缀             | 示例                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

selector 状态命名习惯:

| 状态值	       | 后缀          | 示例                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 布局文件

布局文件应当与对应的Android组件的名字倒置，例如，假设我们为`SignInActivity`创建一个布局文件，文件名应该为`activity_sign_in.xml`.

| 组件        | 类名             | 布局文件名                 |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

有一种稍微不同的情况是，当我们需要在`Adapter`中填充布局的时候，这通常发生在操作`ListView`时，布局文件的名字应当以`item_`开头.

还需要注意到在一些情景中上面这些规则很难被遵守，例如，创建的布局文件将会是其他布局中的一部分，这种情况下我们就用 `partial_`坐为前缀.

#### 1.2.2.3 菜单文件

和布局文件类似，菜单文件也应当对应相应的组件，例如，假设我们定义一个将要在`UserActivity`中使用的菜单，这个文件名应当为`activity_user.xml`
一个好的实践是不要在文件名中包含`menu` 因为所创建的文件就处在`menu`目录中.

#### 1.2.2.4 Values 文件

在`values`文件夹中的资源文件名应当都为__复数__形式, 例如： `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 2 代码指引

## 2.1 Java 语言规则

### 2.1.1 不要忽略异常

永远不要这么做:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_也许你会想不会触发这个条件，或者说处理这个异常并不重要，不过类似上面这样忽略异常就像在代码中埋坑，等着某天其他人来跳.你有原则性的应该处理每一处异常，在特殊的场景中特殊处理._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

也可以看 [这里](https://source.android.com/source/code-style.html#dont-ignore-exceptions).

### 2.1.2 不要捕获 generic exception

不应该这样做:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```
 [这里](https://source.android.com/source/code-style.html#dont-catch-generic-exception)说明了为什么要这样

### 2.1.3 不要使用 finalizers

_我们不使用finalizers.finalizers的调用没有保证，不知道何时会被调用，甚至会不会被调用.大部分情况下，你可以使用好的异常捕获来代替finalizers的使用.如果你一定要用，定义一个叫`close()` 或类似名字的方法，在该方法的文档中详细说明这个方法需要被调用.参考`InputStream`._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))



### 2.1.4 完全限定引用

这样不好: `import foo.*;`

这样好: `import foo.Bar;`

[这里](https://source.android.com/source/code-style.html#fully-qualify-imports)查看更多

## 2.2 Java 风格规则

### 2.2.1 成员变量定义和命名
成员变量需要定义在__文件开头__，且应当遵循以下规则.

* 私有，非静态以 __m__ 开头.
* 私有, 静态以 __s__ 开头.
* 其他变量以小字母开头.
* 常量全部大写下划线分隔单词 ALL_CAPS_WITH_UNDERSCORES.

例如:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### 2.2.3 单词首字母大写

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.4 使用空格缩进

代码块缩进用 __4 空格__ :

```java
if (x == 1) {
    x++;
}
```

分行缩进使用 __8 空格__ :

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.5 使用标准括号风格

左大括号跟随在起始行末尾而不要另起一行.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

只有条件和内容在同行显示时才不需要括号，例如：
```java
if (condition) body();
```

这样__不好__:

```java
if (condition)
    body();  // bad!
```

### 2.2.6 注解

#### 2.2.6.1 注解实践

According to the Android code style guide, the standard practices for some of the predefined annotations in Java are:

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.

* `@SuppressWarnings`: The @SuppressWarnings annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotation guidelines can be found [here](http://source.android.com/source/code-style.html#use-standard-java-annotations).

#### 2.2.6.2 注解风格

__类，方法，构造方法__

在注解运用在类，方法，构造方法上时，应当在文档代码块的下面，同时要求__一个注解一行__ .

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__变量__

在变量上使用注解时所有的注解应该写在同一行，除非超过了单行最大长度.

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.7 限制变量范围

_局部变量范围应该限制在最小范围上，这样可以增加可读性和可维护性，减少出错的可能，每一个变量都应在该变量全部使用范围内的最小代码块中声明._

_局部变量应当在第一次使用时声明，而且绝大部分情况下都应该在声明时完成初始化.如果没有足够的信息来完成初始化工作，那就应该推迟声明直到获取到这些足够初始化的信息._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.8 import顺序

如果你在使用像Android Studio这样的IDE就不需要担心，因为IDE会帮你搞定.如果没有的话看看下面这些：

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax
4. Same project imports

To exactly match the IDE settings, the imports should be:

* Alphabetically ordered within each grouping, with capital letters before lower case letters (e.g. Z before a).
* There should be a blank line between each major grouping (android, com, junit, net, org, java, javax).

More info [here](https://source.android.com/source/code-style.html#limit-variable-scope)

### 2.2.9 Logging guidelines

使用`Log`类提供的方法打印日志.

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

一般来说我们在类文件中使用类名定义一个`static final` 的Tag变量.

```java
public class MyClass {
    private static final String TAG = MyClass.class.getSimpleName();

    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```

VERBOSE 和 DEBUG 日志信息__必须__在release版中禁用. 同样的INFORMATION, WARNING 和 ERROR 日志也推荐禁用掉，除非你想在release版中输出可以用来帮助解决问题的日志. 如果选择不禁用，就必须保证输出的log信息不会泄露用户的隐私信息，例如邮箱地址，用户Id等.

只有在Debug中才会输出的日志示例:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```

### 2.2.10 Class member顺序
对于成员变量顺序而言并没有唯一正确的解决方案，但是有逻辑的一致性的排序将会提高代码的可读性.
推荐使用以下顺序：

1. 常量
2. 成员变量
3. 构造方法
4. Override 方法 和 回调方法 (public 或者 private)
5. 公共方法
6. 私有方法
7. 内部类和接口

Example:

```java
public class MainActivity extends Activity {

	private String mTitle;
    private TextView mTextViewTitle;

    public void setTitle(String title) {
    	mTitle = title;
    }

    @Override
    public void onCreate() {
        ...
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```
如果一个类继承自__Android component__比如 Activity 或 Fragment, 把override方法按照该__组件的生命周期排序__是个不错的方法. 例如, 有一个Activity 实现了 `onCreate()`, `onDestroy()`, `onPause()` 和 `onResume()`, 那么正确的排序是:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.11方法中参数排序

在android开发过程中，我们经常会定义一个方法使用`Context`作为参数传入.当我们在写这种方法时候，  __Context__ 必须为 __第一个__ 参数.
一个相反的例子是__callback__接口应该作为__最后一个__参数.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 String constants, naming, and values

在Android SDK中有很多元素比如 `SharedPreferences`, `Bundle`, or `Intent` 使用了 key-value 键值对的方式，所以即使一个很小的应用你都会写上很多String常量.

当使用这些组件时 __必须__ 定义一个 `static final` 常量并且以下面这些标示开头.

| 元素类型            | 前缀 |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

注意到Fragment的参数（ `Fragment.getArguments()` ）也是Bundle类型.因为这相当常用，所以我们还是使用了单独定义的名称.
Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Arguments in Fragments and Activities

当数据通过 `Intent` 或者 `Bundle` 传递给 `Activity ` 或 `Fragment` 时 , keys必须遵循上面的规则.

当`Activity` 或 `Fragment` 需要传入参数时, 它应当提供一个包含简化了创建 `Intent` 或 `Fragment` 的 `public static` 方法.

在Activities中该方法通常叫做 `getStartIntent()`:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```
对Fragments来说该方法名叫 `newInstance()`，在其中含有参数的创建.
```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment;
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: 这些方法应当在 `onCreate()` 方法之前.

__Note 2__: 如果我们提供了上述方法, 那么这些参数的Keys应该使用 `private` 修饰符，因为这些常量没有必要对外暴露.

### 2.2.15 单行长度限制

一行长度不应该超过 __100 个字符__. 如果太长了通常有一下两种方法减小长度:

* 提取变量或者方法 (推荐).
* 分多行显示.

两个例外:

* 无法分行显示, 例如很长的Url.
* `package` 和 `import`.

#### 2.2.15.1 分行策略
有很多分行的解决方法，然而并没有什么精确的程式规定如何分行，但是依然有些可以应用在大部分情况下的规则可以参考.

__操作符__

在操作符之前换行:

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
```

__赋值操作特例__

但是 `=` 是个例外, 这个时候应该在此之后换行.

```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__链式调用__

当许多方法在同一行调用时（例如建造者模式）每一个调用都应该单独成行并且在 `.` 之前换行.

```java
Picasso.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
Picasso.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__多参数__

当一个方法参数很多或者很长时，每一个参数也应单独成行并且 `,` 之后换行.

```java
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(context,
        "http://ribot.co.uk/images/sexyjoe.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

## 2.3 XML 风格规则

### 2.3.1 使用自闭标签

当一个XML元素不包含内容时, __必须__ 使用自闭标签.

This is good:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 资源命名

资源 ID 和 名称写成小写单词间用下划线连接.

#### 2.3.2.1 ID 命名

id必须以元素的名称为开头. 例如:


| 元素            | 前缀            |
| -----------------  | ----------------- |
| `TextView`           | `text_`             |
| `ImageView`          | `image_`            |
| `Button`             | `button_`           |
| `Menu`               | `menu_`             |

Imageview:

```xml
<ImageView
    android:id="@+id/image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings

字符串名称应该以所属分类开头. 例如 `registration_email_hint` or `registration_name_hint`. 如果一个字符 __不属于__ 任何分类, 则应该采用以下规则:


| 前缀             | 描述                           |
| -----------------  | --------------------------------------|
| `error_`             | 一个错误信息                     |
| `msg_`               | 一个常规的信息         |
| `title_`             | 标题，比如对话框标题          |
| `action_`            | 保存或创建之类的动作  |



#### 2.3.2.3 Styles 和 Themes

和其他资源不同 style 名称写成 __驼峰式命名__ .

### 2.3.3 属性顺序

一般情况下都应将类似的属性写在一块， 像下面这种是比较好的方式:

1. View Id
2. Style
3. Layout width and layout height
4. 其他 layout 属性, 按字母排序
5. 其他剩下的属性, 按字母排序

## 2.4 测试风格规则

### 2.4.1 单元测试

测试类应该与被测试类名字相对于, 然后在后面跟上 `Test`. 举个栗子, 如果我们为 `DatabaseHelper` 这个类创建测试类, 这个测试类应该命名为 `DatabaseHelperTest`.

 `@Test`标注的方法名应该与被测试的方法名字相对应，然后跟上测试的预置条件或者期望行为.

* 模板: `@Test void methodNamePreconditionExpectedBehaviour()`
* 示例: `@Test void signInWithEmptyEmailFails()`

如果测试方法名很清晰明了那么预置条件或者期望行为的描述并不是必须的.
有时候一个类有非常多的方法，同时每个方法又有很多测试方法，这种情况下，推荐把不同的测试方法分开放到不同的测试类中.比如，有一个 `DataManager` 包含很多方法，我们可以把测试类分成`DataManagerSignInTest`, `DataManagerLoadUsersTest`等等. Generally you will be able to see what tests belong together because they have common [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).

### 2.4.2 Espresso 测试

通常 Espresso测试类目标都是Activity, 因此在被测试的Activity后面加上 `Test`, 比如 `SignInActivityTest`

在用 Espresso API 的时候通常也是各个方法独占一行.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```

# License

```
Copyright 2015 Ribot Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
