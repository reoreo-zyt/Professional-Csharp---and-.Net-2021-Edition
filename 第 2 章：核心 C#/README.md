> 这一章有什么？
>
> ➤ 顶级语句
>
> ➤ 声明变量
>
> ➤ 目标类型的新表达式
>
> ➤ 可空类型
>
> ➤ C#预定义类型
>
> ➤ 程序流程
>
> ➤ 命名空间
>
> ➤ 字符串
>
> ➤ 注释和文档
>
> ➤ C#预处理器指令
>
> ➤ 指南和代码规范



> 本章中的代码下载
>
> 本章的源代码可以在www.wiley.com的图书页面上找到。单击“下载”链接。该代码也可以在https://github.com/ProfessionalCSharp/ProfessionalCSharp2021 的目录1_CS/CoreCSharp中找到。
>
> 本章的代码分为以下主要例子：
>
> ➤ 顶级语句
>
> ➤ 命令行参数
>
> ➤ 变量作用域示例
>
> ➤ 可空值类型
>
> ➤ 可空引用类型
>
> ➤ 程序流程
>
> ➤ Switch声明
>
> ➤ Switch表达式
>
> ➤ For循环
>
> ➤ 字符串示例
>
> 所有的示例项目都启用了可为空的引用类型。



## C#基础

现在您更了解了C#可以做什么，您需要知道如何使用它。本章通过提供对C#编程的基本原理的基本理解，为您提供了一个很好的开端。在本章结束时，您将知道足够多的C#知识来编写简单的程序（尽管不使用继承或其他面向对象的特性，这些特性将在后面的章节中介绍）。

前一章解释了如何使用.NET CLI创建一个“Hello,World!”的应用程序。本章主要介绍了C#语法。首先，下面是一些关于语法的一般信息：

➤ 语句以分号`;`结尾，并且可以继续超过多行，而不需要延续字符。

➤ 语句可以使用花括号`{}`连接到块中。

➤ 单行注释以两个正斜杠字符开头`//`。

➤ 多行注释以斜杠和星号`/*`开始，最后以相同的反向组合`*/`结束。

➤ C#区分大小写。名为`myVar`和`MyVar`的变量是两个不同的变量。



### 顶级语句

C#9的一个新特性是顶级语句。您可以创建简单的应用程序，而不需要定义命名空间、声明类和定义Main方法。只有一句话：“Hello,World!”应用程序可以如下：

```c#
System.Console.WriteLine("Hello World!");
```

让我们增强这个一行应用程序，以打开定义了Console类的命名空间。使用using指令来导入系统命名空间，您可以使用Console类，而无需使用命名空间作为前缀：

```c#
using System;
Console.WriteLine("Hello World!");
```

因为WriteLine是Console类的一个静态方法，所以甚至可以使用static指令来打开Console类：

```c#
using static System.Console;
WriteLine("Hello World!");
```

在幕后，使用顶级语句，编译器使用Main方法创建一个类，并将顶级语句添加到Main方法中：

```c#
using System;
class Program
{
    static void Main()
 {
 Console.WriteLine("Hello, World!");
 }
}
```

> 注意：本书中的许多示例都使用顶级语句，因为这个特性对于小示例应用程序非常有用。这个特性也可以在小型微服务中实际使用，您现在可以在几行代码中编写，并且在类似脚本的环境中使用C#时也可以使用。



### 变量

C#提供了声明和初始化变量的不同方法。变量具有可以随时间变化的类型和值。在下一个代码片段中，变量s1的类型是string，变量名称左侧为类型声明，它被初始化为一个新的字符串对象，其中字符串文字“Hello,World!”被传递给构造函数。因为字符串类型经常被使用，通常不使用创建一个新的字符串对象，而是将字符串“Hello,World!”直接赋值给变量(如变量s2所示)。

C# 3定义了带有类型推理的var关键字，它也可以用来声明一个变量。在这里，需要的类型在右边，左侧将从中推断出类型。由于编译器从字符串文字“Hello，World”中创建了一个字符串对象，s3和s1、s2一样，是一个类型安全的强定义字符串。

C# 9提供了另一种新语法，用目标类型的新表达式声明和初始化一个变量。而不是写`new string("Hello,World!")`，如果已知的类型在左边，只使用`new string("Hello,World!")`就足够了，您不必指定右侧的类型（代码文件TopLevelStatements/Program.cs）：

```c#
using System;
string s1 = new string("Hello, World!");
string s2 = "Hello, World!";
var s3 = "Hello, World!";
string s4 = new("Hello, World!");
Console.WriteLine(s1);
Console.WriteLine(s2);
Console.WriteLine(s3);
Console.WriteLine(s4);
//...
```

> 注意：使用var关键字或目标类型的新表达式在左侧声明该类型通常只是一个编程风格问题。在幕后，会生成相同的代码。var关键字从c#3开始就可用了，通过在实例化对象时在左侧和声明类型的左侧定义类型，减少了需要编写的代码量。使用var关键字，您只需要在右侧有该类型。但是，var关键字不能用于类型的成员。在c#9之前，您必须使用类成员编写两次类型；现在可以使用目标类型的new。目标类型的new可以与局部变量一起使用，您可以在前面的带有变量s4的代码片段中看到。这并不会使var关键字无用；它仍然有它的优点



### 命令行参数

当您在启动程序传递值时，变量args会自动用顶级语句自动声明。在下面的代码片段中，使用foreach语句，可以访问变量args以遍历所有命令行参数并在控制台上显示值(代码文件CommandLineArgs/Program.cs)：

```c#
using System;
foreach (var arg in args) {
 Console.WriteLine(arg);
}
```

使用.NET CLI运行应用程序，您可以使用dotnet run运行——然后将参数传递给程序。需要添加`--`，以免将.NE TCLI的参数与应用程序的参数混淆：

```bash
 dotnet run -- one two three
```

当您运行这个操作时，您会看到控制台上的字符串one two three。

在创建自定义Main方法时，需要声明该方法以接收字符串数组。您可以为变量选择一个名称，但名为args的变量被普遍使用，这就是为使用顶级语句自动生成的变量选择这个名称的原因：

```c#
using System;
class Program
{
 static void Main(string[] args)
 {
 foreach (var arg in args)
 {
 Console.WriteLine(arg);
 }
 }
}
```



### 理解变量作用域

变量的作用域是可以从中访问变量的代码区域。通常，确定作用域遵循以下：

➤ 只要类的局部变量在某个作用域内，其字段（也叫成员变量）也在该作用域内

➤ 局部变量存在于表示声明该变量的块语句或方法结束的右花括号之前的作用域内

➤ 在for while或类似语句中声明的局部变量存在于该循环体内

在大型程序中，经常会有对程序的不同部分的不同变量使用了相同的变量名。只要变量的范围被限制在程序中完全不同的部分，这样就不会存在歧义。但是，请记住，具有相同名称的局部变量不能在同一作用域中声明两次。例如，您不能这样做：

```c#
int x = 20;
// some more code
int x = 30;
```

请考虑以下代码示例（代码文件VariableScopeSample/Program.cs）：

```c#
using System;
for (int i = 0; i < 10; i++)
{
 Console.WriteLine(i);
} // i goes out of scope here
// We can declare a variable named i again, because
// there's no other variable with that name in scope
for (int i = 9; i >= 0; i--) {
 Console.WriteLine(i);
} // i goes out of scope here.
```

这段代码只是简单地打印出数字从0到9，然后从9到0，使用了两个for循环。需要注意的重要一点是，您在此代码中在同一方法中声明变量i两次。您可以这样做，因为i在两个单独的循环中（作用域中）声明，所以每个i变量都是它自己循环的本地变量。

下面是另一个例子(代码文件VariableScopeSample2/Program.cs)：

```c#
int j = 20;
for (int i = 0; i < 10; i++)
{
 int j = 30; // Can't do this — j is still in scope
 Console.WriteLine(j + i);
}
```

如果您试图编译这个，您会得到一个错误如下：

```bash
error CS0136: A local or parameter named 'j' cannot be declared in this scope because 
that name is used in an enclosing local scope to define a local or 
parameter
```

这是因为在for循环开始之前定义的变量j仍然在for循环的作用域内，在Main方法（从编译器创建）完成执行之前不会超出作用域。编译器无法区分这两个变量，因此它不允许声明第二个变量。

甚至在for循环结束后将变量j声明在for循环之外也没有帮助。编译器将在作用域的开头移动所有变量声明，无论您在哪里声明它。



### 常量

对于从不更改的值，您可以定义一个常量。对于常量值，您可以使用const关键字。

使用const关键字声明的变量，编译器将每次出现中的变量替换为常数指定的值。

在类型之前用const关键字指定一个常量：

```c#
const int a = 100; // This value cannot be changed.
```

编译器将所有带有该值的本地字段替换。这种行为在版本控制方面非常重要。如果使用库声明一个常量并使用应用程序中的常量，则需要重新编译应用程序以获得新值；否则，库可能具有与应用程序具有不同的值。正因为如此，最好只使用那些永远不会改变的值，即使在未来的版本中也是如此。

常量具有以下特征：

➤ 常量必须在声明时初始化，指定值后，就不能再改写了。

➤ 常量的值必须能在编译时用于计算。因此，不能从变量中提取的值来初始化常量。如果这么做，应该只使用只读字段（详见第 3 章）

➤ 常量总是隐式静态的。但注意，不允许在常量声明中包含static

在程序中使用常量的好处：

➤ 常数通过用可读的值易于理解的名称替换难以理解的数字和字符串，使您的程序更容易阅读。

➤ 常量有助于防止程序中的错误。如果您试图为程序中的某个常量分配另一个值，而不是在声明该常量的地方，则编译器会标记该错误。

> 注意：如果多个实例可能有不同的值，但初始化后值不会改变，则可以使用只读关键字。这将在第3章“类、记录、结构和元组”中进行讨论。



### 具有顶级语句的方法和类型

您还可以使用顶级语句向同一文件中添加方法和类型。在下面的代码片段中，名为Method的方法将在方法声明和实现之后定义并调用该方法(代码文件TopLevelStatements/Program.cs)：

```c#
//...
void Method()
{
 Console.WriteLine("this is a method");
}
Method();
//...
```

该方法可以在使用之前或之后进行声明。类型可以添加到同一个文件中，但需要在顶级语句之后指定它们。通过以下代码片段，类book被指定为包含一个Title属性和ToString方法。在声明类型之前，将创建一个新的实例并分配给变量b1，将设置Title属性的值，并将该实例写入控制台。当对象作为参数传递给WriteLine方法时，会调用Book类的ToString方法：

```c#
Book b1 = new();
b1.Title = "Professional C#";
Console.WriteLine(b1);

class Book
{
  public string Title { get; set; }
  public override string ToString() => Title;
}
```

> 注意：在第3章中详细说明了创建和调用方法和定义类。

> 注意：所有顶级语句都需要在一个文件中。否则，编译器将不知道从哪里开始。如果使用顶级语句，则容易查找它们，例如将它们添加到Program.cs文件中。您不希望在包含多个文件的列表中搜索顶级语句。



## 可空类型

对于C#的第一个版本，值类型不能有空值，但总是可以将null分配给引用类型。第一个变化发生在c#2和可空值类型的设计上。c#8对引用类型进行了更改，因为在.NET中发生的大多数异常类型都为NullReferenceException。当调用引用的成员时，会发生这些异常。为了减少这些问题并获得编译器错误，在c#8中引入了可为空的引用类型。

本节包括可为空的值类型和可为空的引用类型。语法看起来很相似，但在程序幕后却非常不同



### 可空值类型

对于像int这样的值类型，您不能将其分配为null。这可能会导致映射到数据库或其他数据源时的困难，如XML或JSON。使用引用类型反而会导致额外的开销：一个对象被存储在堆中，当它不再被使用时，垃圾收集需要清理它。额外的，你可以使用 `?` 与类型定义一起使用，这样允许分配为null

```c#
int? x1 = null;
```

编译器将此操作为Nullable<T>类型：

```c#
Nullable<int> x1 = null;
```

Nullable<T>不会增加引用类型的开销。这仍然是一个结构体（一个值类型），但添加了一个布尔标志来指定该值是否为null。

下面的代码片段演示了使用可空值类型和分配不可空值。变量n1是分配值null的空int。可空值类型定义属性HasValue，该属性可用于检查变量是否分配了值。使用“值”属性，您可以访问其值。这可用于将该值分配给不可为空的值类型。不可空值始终可以分配给可空值类型；这始终会成功（代码文件NullableValueTypes/Program.cs)：

```c#
int? n1 = null;

if (n1.HasValue)
{
    int n2 = n1.Value;
}
int n3 = 42;
int? n4 = n3;

Nullable<int> n5 = null;
int n6 = n5.GetValueOrDefault();

Console.WriteLine(n1);// 
Console.WriteLine(n3);// 42
Console.WriteLine(n4);// 42
Console.WriteLine(n5);// 
Console.WriteLine(n6);// 0
```



### 可空引用类型

可空引用类型的目标是减少可空引用异常类型的异常，这是在.NET应用程序中发生的最常见的异常。总是有一个指导原则，应用程序不应该抛出这样的异常，应该总是检查null，但是如果没有编译器的帮助，这样的问题很容易被遗漏。

若要从编译器中获得帮助，您需要打开可为空的引用类型。因为此特性破坏了对现有代码的更改，所以您需要显式地打开它。在项目文件（项目文件NullableReferenceTypes.csproj）中指定Nullable元素并设置为enabled值：

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
 <OutputType>Exe</OutputType>
 <TargetFramework>net5.0</TargetFramework>
 <Nullable>enable</Nullable>
 </PropertyGroup>
</Project>
```

现在，不能将null分配给引用类型。当您编写这个启用了可为空的代码时。

```c#
string s1 = null; // compiler warning
```

您将得到编译器警告“CS8600: Converting a null literal or a possible null value to non-nullable type.”要将null分配给string，需要使用类似问号的空值类型声明类型：

```c#
string? s1 = null;
```

当使用可为空的s1变量时，需要确保在调用方法或将其分配给不可为空的字符串之前验证是否为null；否则，会生成编译器警告：

```c#
string s2 = s1.ToUpper(); // compiler warning
```

解决办法是，您可以使用空条件操作符`?`，调用该方法之前检查null，该操作符仅在对象不是空时才调用该方法。结果不能写入非空字符串。如果s1为null，则正确表达式的结果可以为空：

```c#
string? s2 = s1?.ToUpper();
```

你可以使用合并操作符 `??`要在返回值为空的情况下定义一个不同的返回值。使用下面的代码片段，将返回一个空字符串，以防表达式在`??`返回空值。正确表达式的完整结果现在被写入变量s3，它永远不能为空。如果s1不是空，则要么是s1字符串的大写版本，如果s1为null，则是空字符串：

```c#
string s3 = s1?.ToUpper() ?? string.Empty;
```

您还可以使用if语句来验证变量是否为空，而不是使用这些运算符。对于以下代码片段中的if语句，c#模式不用于验证s1是否为null。只有当s1不是空时，才会调用if语句所覆盖的块。这里不需要使用零条件运算符来调用ToUpper的方法：

```c#
if (s1 is not null)
{
 string s4 = s1.ToUpper();
}
```

当然，它也可以使用不等运算符`!=`：

```c#
if (s1 != null)
{
 string s5 = s1.ToUpper();
}
```

> 注意：操作符详见第5章“操作符和强制类型转换”

对于类型的成员，使用可为空的引用类型也很重要，如以下代码片段中具有Title、Publisher属性的book类所示。标题以不可为空的字符串类型声明，因此，在创建Book类的新对象时，需要对其进行初始化。它已经用Book类的构造函数初始化了。Publisher属性被允许为null，因此它不需要初始化(代码文件NullableReferenceTypes/Program.cs)：

```c#
class Book
{
 public Book(string title) => Title = title;
 
 public string Title { get; set; }
 public string? Publisher { get; set; }
}
```

当您声明Book类的一个变量时，该变量可以声明为空值(b1)，或者它需要一个使用构造函数(b2)进行声明的Book对象。可以将Title属性分配给不可为空的字符串类型。使用Publisher属性，您可以将其分配给可空字符串或使用前面所示的操作符：

```c#
Book? b1 = null;
Book b2 = new Book("Professional C#");
string title = b2.Title;
string? publisher = b2.Publisher;
```

在可空值类型的幕后，在幕后使用类型 Nullable<T>。非空引用类型并非这样。相反，编译器会向这些类型添加注释。可空引用类型具有关联的Nullable属性。这样，可以将可空的引用类型与库一起使用，以可空性注释参数和成员。当库与新的应用程序一起使用时，智能感知可以提供有关方法或属性是否可以为空的信息，并且编译器会相应地执行编译器警告。使用较老版本的编译器(早于c#8)，仍然可以以使用非注释库的方式使用库。编译器只是忽略了它不知道的属性。

> 注意：本书的几乎所有示例都配置为启用可为空的引用类型。在.NET5中，几乎所有的基类库都用可空的引用类型完全注释。这有助于获取关于参数需要什么和返回什么的信息。这里一个有趣的方面是微软在决定可空性时所做的选择。从该对象中返回的字符串。ToString方法最初记录了重写此方法不应该返回null。.NET团队回顾了不同的实现：一些覆盖此方法的微软团队返回了null。因为用法与文档不同，微软决定声明该对象。ToString方法返回`string?`，这允许它返回null。重写此方法后，可以更严格并返回字符串。覆盖方法在第4章“c#面向对象编程”中详细解释。
>
> 因为在使用现有应用程序启用此特性时，可为空的引用类型是一个破坏性的更改，所以为了允许缓慢迁移到这个新特性，您可以使用预处理器指令#nullable来打开或关闭它，并将其从项目文件恢复到设置。这将在“c#预处理器指令”一节中进行讨论。



## 使用预定义类型

现在您已经看到了如何声明变量和常量，并了解了具有可空性的极其重要的增强，让我们仔细研究一下c#中可用的数据类型。

数据类型的c#关键字——如int、short和string——从编译器映射到.NET数据类型。例如，当您在c#中声明一个int时，您实际上是在声明一个.NET结构体：System.Int32。所有的原始数据类型都提供了可以被调用的方法。例如，要将inti转换为字符串，您可以编写以下内容：

```c#
string s = i.ToString();
```

我应该强调的是，在这种语法的背后，类型实际上是存储为原始类型的，因此，原始类型由.NET结构表示绝对没有性能问题。

下面的部分回顾c#中识别为内置类型的类型。列出了每个类型及其定义和相应的.NET类型的名称。我还向您展示了一些例外——一些重要的数据类型，它们只有它们的.NET类型可用，并且没有特定的c#关键字。

让我们从预定义值类型开始，比如整数、浮点数、字符和布尔值。



### 整数类型

c#支持使用不同位数的整数类型，并且在只支持正值的类型或具有负值和正值范围的类型之间有所不同。byte类型和sbyte类型使用八个字节位。byte类型允许0到255的值，只有正值，而sbyte中的s表示使用符号；该类型支持-128到127的值，这是8位可能的值。

short类型和ushort类型使用16位。short类型覆盖的范围从-32768到32767。对于ushort类型，u是表示无符号的，它覆盖了0到65,535。类似地，int类型是一个有符号的32位整数，而uint类型是一个无符号的32位整数。long和ulong有64位可用。在幕后，c#关键字sbyte、byte、int和long映射到System.SByte, System.Int16, System.Int32, 和System.Int64。其余将映射到 System.Byte, System.UInt16, System.UInt32, and System.UInt64。底层的.NET类型清楚地列出了在该类型的名称中使用的位数。

要检查类型的最大值和最小值，可以使用MaxValue 和 MinValue属性。



### 大整数

如果您需要一个比long类型中可用的64位的值更大的数字表示，您可以使用BigInteger类型。这个结构体对比特数没有限制，并且可以在没有足够的可用内存之前继续增长。这种类型没有特定的c#关键字，您需要使用BigInteger。因为这种类型可以不断增长，所以MaxValue 和 MinValue属性不可用。这种类型提供了内置的计算方法，如 Add, Subtract, Divide, Multiply, Log, Log10, Pow等。



### 本机整数类型

对于int、short和long，字节位取决于应用是32位还是64位的。这与用C++定义的整数不同。c#9对于特定于平台的值有了新的关键字：nint和nuint（分别是本机整数和本机无符号整数）。在64位应用程序中，这些整数类型使用64位，而在32位应用程序中，仅使用32位。这些类型对于直接内存访问非常重要，这在第13章“托管和非托管内存”中涉及。



### 数字分隔符

为了更好地提高数字的可读性，您可以使用数字分隔符。您可以向数字中添加下划线，如下的代码片段所示。在此代码片段中，0x前缀用于指定十六进制值(代码文件DataTypes/Program.cs)：

```c#
long l1 = 0x_123_4567_89ab_cedf;
```

作为分隔符使用的下划线只会被编译器忽略。这些分隔符有助于提高可读性，并且不添加任何功能。使用前面的示例，从右边读取，每16位（或4个十六进制字符）就会添加一个数字分隔符。与此相比，这更易读：

```c#
long l2 = 0x123456789abcedf;
```

当然，因为编译器忽略了下划线，所以您自己负责可读性。你可以把下划线放在任何位置，这可能对可读性没有真正的帮助：

```c#
long l3 = 0x_12345_6789_abc_ed_f;
```

使用任何位置都是很有用的，它允许不同的用例，例如使用十六进制或八进制值，或者分离协议所需的不同位，如下一节所示。



### 二进制值

除了提供数字分隔符外，c#还使为整数类型分配二进制值变得容易。使用0b文字，只允许分配0和1的值，如下(代码文件DataTypes/Program.cs)

```c#
uint binary1 = 0b_1111_1110_1101_1100_1011_1010_1001_1000;
```

前面的代码片段使用了一个有32位可用的无符号int。数字分隔符有助于提高使用二进制值的可读性。这个代码片段每4位分离一次。记住，你也可以用十六进制符号来写这个

```c#
uint hex1 = 0xfedcba98;
```

每3位使用分隔符有助于使用八进制表示法，其中字符在0（000二进制）和7（111二进制）之间使用。

```c#
uint binary2 = 0b_111_110_101_100_011_010_001_000;
```

如果您需要定义一个二进制协议—例如，2位定义最右边的部分，在下一节中定义6位，2乘以4位来完成16位—您可以按照这个协议放置分隔符：

```c#
ushort binary3 = 0b1111_0000_101010_11;
```

> 注意：有关使用二进制数据的其他信息，请阅读第5章。



### 浮点类型

c#还根据IEEE 754标准指定了具有不同位数的浮点类型。Half类型(.NET5的新类型)使用16位，float类型(对应.NET Single)使用32位，double类型（Double）使用64位。对于所有这些数据类型，符号都使用了1位。根据类型的不同，10到52位用于有效值，5到11位用于指数。详情如下表所示：

| c#关键字 | .net类型      | 描述               | 有效位 | 指数位 |
| -------- | ------------- | ------------------ | ------ | ------ |
|          | System.Half   | 16位，单精度浮点数 | 10     | 5      |
| float    | System.Single | 32位，单精度浮点数 | 23     | 8      |
| double   | System.Double | 64位，双精度浮点数 | 52     | 11     |

 当您赋值时，如果您硬编码一个非整数（如12.3），编译器将假定它是double。要指定该值为float，请附加字符F（或f）：

```c#
float f = 12.3F;
```

对于decimal类型(.NET结构Demical)，.NET有一个高精度的浮点类型，使用128位，可用于财务计算。对于128位，1用于符号，96表示整数。其余的位指定了一个比例因子。要指定您的数字是decimal类型，而不是double float或者整数，您可以将M(或M)字符附加到该值中：

```c#
decimal d = 12.30M;
```



### 布尔类型

您可以使用c# bool类型来包含true或false的布尔值。您不能隐式地将bool值与整数值转换。如果将变量（或函数返回类型）声明为bool，则只能使用true和false的值。如果你试图使用零表示false，使用非零值表示true，你会得到一个错误



### 字符类型

.NET字符串由两字节的字符组成。c#关键字char映射到.NET类型Char。使用单引号，例如`'A'`，将创建一个字符。使用双引号，将创建一个字符串。

除了将字符表示为字符文字外，还可以用四位数十六进制Unicode值(例如`'\u0041'`）、带有强制类型转换的整数值（例如`(char)65`）或十六进制值（例如`'\x0041'`)）表示它们。您也可以用转义序列来表示它们，如下表所示：

| 转义序列 | 字符       |
| -------- | ---------- |
| `\'`     | 单引号     |
| `\''`    | 双引号     |
| `\\`     | 反斜杠     |
| `\0`     | 空         |
| `\a`     | 警告       |
| `\b`     | 退格       |
| `\f`     | 换页       |
| `\n`     | 换行       |
| `\r`     | 回车       |
| `\t`     | 水平制表符 |
| `\v`     | 垂直制表符 |



### 数字的字面值

在前面的章节中，已经展示了数字的字面值。让我们在下表中总结一下它们：

| 字面值 | 位置 | 说明                         |
| ------ | ---- | ---------------------------- |
| U      | 后缀 | 无符号int                    |
| L      | 后缀 | long                         |
| UL     | 后缀 | 无符号long                   |
| F      | 后缀 | float                        |
| M      | 后缀 | decimal                      |
| 0x     | 前缀 | 十六进制数；允许有从0到F的值 |
| 0b     | 前缀 | 二进制数；只允许使用0和1     |
| true   | NA   | 布尔值                       |
| false  | NA   | 布尔值                       |



### 对象类型

除了值类型，c#关键字还定义了两个引用类型：映射到Object类的object关键字和映射到String类的string关键字。string类型将在本章后面的“使用字符串”一节中讨论。Object类是所有引用类型的根类型，可用于两个目的：

➤ 您可以使用object引用来绑定任何特定子类型的对象。例如，在第5章中，您将看到如何使用object类型把堆栈中的值对象装箱，再移动到堆中。object引用在反射中也很有用，此时必须有代码处理类型未知的对象。

➤ 对象类型实现了许多基本的通用方法，其中包括Equals、GetHashCode、GetType和ToString。用户定义的类需要使用一种面向对象技术——重写，这将在第4章中讨论。例如，当您重写ToString时，您为类配备了一种智能地提供自身的字符串表示的方法。如果您没有在类中为这些方法提供您自己的实现，那么编译器将获取object类型的实现。



## 控制程序流

本节将介绍该语言最基本的重要语句：允许您控制程序流程的语句，而不是按照它在程序中出现的顺序执行每行代码。使用if和switch语句等条件语句，您可以根据是否满足某些条件对代码进行分支。您可以使用for、while和foreach语句在循环中执行重复语句。



### if 语句

使用if语句，您可以在圆括号内指定一个表达式。如果表达式返回true，则执行用花括号指定的块的代码。如果条件不为真，您可以使用else if来检查其他条件是否为真。else if可以重复检查更多的条件。如果if指定的表达式和所有else if表达式均不计算为true，则执行else块的代码。

使用以下代码片段，将从控制台读取一个字符串。如果输入了空字符串，则将调用if语句后面的代码块。string的方法IsNullOrEmpty在string是null或空值时返回true。当输入的长度小于5个字符时，调用使用else if语句指定的块。在所有其他情况下，例如，输入长度为5个或更多字符，将调用else块(代码文件ProgramFlow/Program.cs)：

```c#
Console.WriteLine("Type in a string");
string? input = Console.ReadLine();

if (string.IsNullOrEmpty(input))
{
 Console.WriteLine("You typed in an empty string.");
}
else if (input?.Length < 5)
{
 Console.WriteLine("The string had less than 5 characters.");
}
else
{
 Console.WriteLine("Read any other string");
}
Console.WriteLine("The string was " + input);
```

> 注意：对于if语句，如果条件分支只有一句，则不需要使用花括号；

使用if语句，else if和else是可选的。如果您只需要基于一个条件调用代码块，而如果不满足这个条件不调用代码块，则可以只使用if。



### 模式匹配运算符

> [模式 - C# 参考 | Microsoft Docs](https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/patterns)

c#特性之一是模式匹配，您可以将它与if语句和is运算符一起使用。前面的部分“可空引用类型”包含了一个使用if语句和`is not null`的示例。

下面的代码片段将接收到的object类型的值与null比较，使用null在常量模式下比较参数，并抛出ArgumentNullException。else if中，类型模式用于检查变量o是否为Book类型。如果是这样，则将变量o分配给变量b。因为变量b是Book类型，所以使用b可以访问由Book类型指定的Title属性(代码文件ProgramFlow/Program.cs)：

```c#
void PatternMatching(object o)
{
 if (o is null) throw new ArgumentNullException(nameof(o));
 else if (o is Book b)
 {
 Console.WriteLine($"received a book: {b.Title}");
 }
}
```

> 注意：在本例中，为了抛出ArgumentNullException，我们使用了nameof表达式。nameof表达式获取参数的名称，例如，变量o，并将其作为字符串传递。throw new ArgumentNullException(nameof(o))；解析为与 throw new ArgumentNullException("o")相同的代码。但是，如果将变量o重命名为不同的值，则重构功能可以自动重命名使用nameof表达式指定的变量。如果在重命名变量时没有更改nameof的参数，则会导致一个编译器错误。如果没有nameof表达式，变量和字符串不容易同步。

下面的代码片段中显示了关于常数和类型模式的更多示例：

```c#
if (o is 42) // const pattern
if (o is "42") // const pattern
if (o is int i) // type pattern
```

> 注意：可以使用与is运算符、swich语句和switch表达式匹配的模式匹配。您可以使用不同类别的模式匹配。本章仅涵盖常量、类型、关系模式和模式组合符。更多的模式，如属性模式，元组模式，和递归模式将在第三章提及。



### switch 语句

switch/case语句可用于从一组互斥的分支中选择一个分支执行。它采用了switch关键字包含参数的形式，后面接上一系列case子句。当switch参数中的表达式的计算结果为case子句指定的值之一时，case子句后面的代码将被执行。不需要使用花括号将case语句连接成块；另外，您需要使用break语句标记每个大小写的代码末尾。您还可以在switch语句中包含默认情况，如果表达式不计算其他任何情况，则执行。以下switch语句测试x变量的值（代码文件SwitchStatement/Program.cs）：

```c#
    switch (x)
    {
        case 1:
            Console.WriteLine("integerA = 1");
            break;
        case 2:
            Console.WriteLine("integerA = 2");
            break;
        case 3:
            Console.WriteLine("integerA = 3");
            break;
        default:
            Console.WriteLine("integerA is not 1, 2, or 3");
            break;
    }
```

请注意，case值必须是常量表达式；不允许使用变量。

使用switch语句，可以使用goto在case之间跳转，但必须要有break跳出switch语句，如下：

```c#
goto case 3;
```

如果实现与多个case完全相同，则可以在指定实现之前指定多个case：

```c#
switch(country)
{
 case "au":
 case "uk":
 case "us":
 language = "English";
 break;
 case "at":
 case "de":
 language = "German";
 break;
}
```



### 使用switch语句的模式匹配

模式匹配也可以与switch语句一起使用。下面的代码片段显示了常量、类型以及关系模式。方法SwitchWithPatternMatching接收一个object类型的参数。case null是一个常数模式，比较o是否为null。在接下来的三种情况下，我们指定了一个类型模式。`case int i`使用一个类型模式来创建变量i（如果变量o是int类型），与when子句结合使用。when子句使用一个关系模式来检查它是否大于42。下一个示例匹配所有剩余的int类型。程序中，没有指定应该分配的对象o的变量。如果不需要这个变量，只需要知道它是这种类型，而不需要指定变量。当与Book类型匹配时，将使用变量b。在这里声明一个变量，此变量的类型为Book(代码文件SwitchStatement/Program.cs)

```c#
void SwitchWithPatternMatching(object o)
{
 switch (o)
 {
 case null:
 	Console.WriteLine("const pattern with null");
	break;
 case int i when i > 42
 	Console.WriteLine("type pattern with when and a relational pattern");
     break;
 case int:
 	Console.WriteLine("type pattern with an int");
 	break;
 case Book b:
 	Console.WriteLine($"type pattern with a Book {b.Title}");
 	break;
 default:
 	break;
 }
}
```



### switch表达式

下一个示例显示了一个基于枚举类型的switch。枚举类型基于不同的命名的整数。TrafficLight类型定义了交通灯颜色的不同值（代码文件SwitchExpression/Program.cs）：

```c#
enum TrafficLight
{
 Red,
 Amber,
 Green
}
```

> 注意：第3章更详细地介绍了枚举类型，您可以看到更改基类型和分配不同值。

到目前为止，对于switch语句，您只看到在每种情况下都调用了一些操作。当您使用返回语句从一个方法返回时，您还可以直接从该case下返回一个值，而无需继续使用以下cases。方法NextLightClassic接收一个TrafficLight及其参数，并返回一个TrafficLight的参数。如果通过的交通灯的值为TrafficLight.Green，该方法返回TrafficLight.Amber。当当前指示灯值为TrafficLight.Amber，返回TrafficLight.Red：

```c#
TrafficLight NextLightClassic(TrafficLight light) 
{
 switch (light)
 {
 case TrafficLight.Green:
 	return TrafficLight.Amber;
 case TrafficLight.Amber:
 	return TrafficLight.Red;
 case TrafficLight.Red:
 	return TrafficLight.Green;
 default:
 	throw new InvalidOperationException(); 
 }
}
```

在这种情况下，如果您需要根据不同的选项返回一个值，则可以使用c#8中新的switch表达式。方法NextLight接收并返回与前面显示的方法类似的TrafficLight值。`=>` 标记用于定义返回的内容，而不是使用case关键字。该功能与以前相同，但您只需要更少的代码行：

```c#
TrafficLight NextLight(TrafficLight light) =>
 light switch
 {
  TrafficLight.Green => TrafficLight.Amber,
  TrafficLight.Amber => TrafficLight.Red,
  TrafficLight.Red => TrafficLight.Green,
  _ => throw new InvalidOperationException()
 };
```

如果枚举类型通过使用static指令导入，则可以通过使用不带类型名称的枚举值定义来进一步简化实现：

```c#
using static TrafficLight;
TrafficLight NextLight(TrafficLight light) =>
 light switch
 {
 Green => Amber,
 Amber => Red,
 Red => Green,
 _ => throw new InvalidOperationException()
 };
```

> 注意：还有其他选项需要属性模式或基于元组的模式匹配。这一点详见第3章。

在下一个例子中，使用了多个模式。首先，从控制台检索输入。如果输入字符串1或两个，则使用相同的匹配，使用or组合模式(代码文件SwitchExpression/Program.cs)

```c#
string? input = Console.ReadLine();
string result = input switch
{
 "one" => "the input has the value one",
 "two" or "three" => "the input has the value two or three",
 _ => "any other value"
};
```

使用模式组合，您可以使用and，or，和not关键字。



### for循环

c#提供了四种不同的循环（for, while, do-while, 和 foreach），使您能够重复执行一个代码块，直到满足某个条件。使用for关键字，您遍历一个循环，在执行另一个迭代之前，判定一个特定条件是否为真：

```c#
for (int i = 0; i < 100; i++)
{
 Console.WriteLine(i);
}
```

for语句的第一个表达式是初始化器。在执行第一个循环之前，将对其进行计算。通常，您使用它来初始化一个本地变量作为一个循环计数器。

第二个表达式是条件。这将在for块的每次迭代之前进行检查。如果此表达式的计算结果为true，则会执行该块。如果计算结果为false，for语句将结束，程序将在关闭for主体的花括号之后继续使用下一个语句。

在执行主体之后，将计算第三个表达式，即迭代器。通常，您会增加循环计数器。使用i++，给变量i加一个值1。在第三个表达式之后，再次计算条件表达式，以检查是否应该使用for块进行另一次迭代。

for循环是一个所谓的预测试循环，因为循环条件是在执行循环语句之前计算的；因此，如果循环条件为false，那么循环的内容将根本不会被执行。

嵌套循环并不罕见，内部循环会完全执行一次外环的每次迭代。这种方法通常用于循环遍历多维数组中的每个元素。

```c#
// This loop iterates through rows
for (int i = 0; i < 100; i += 10)
{
 // This loop iterates through columns
 for (int j = i; j < i + 10; j++)
 {
 Console.Write($" {j}");
 }
 Console.WriteLine();
}
```

此示例的输出结果如下：

```bash
0 1 2 3 4 5 6 7 8 9
10 11 12 13 14 15 16 17 18 19
20 21 22 23 24 25 26 27 28 29
30 31 32 33 34 35 36 37 38 39
40 41 42 43 44 45 46 47 48 49
50 51 52 53 54 55 56 57 58 59
60 61 62 63 64 65 66 67 68 69
70 71 72 73 74 75 76 77 78 79
80 81 82 83 84 85 86 87 88 89
90 91 92 93 94 95 96 97 98 99
```



### while 循环

就像for循环一样，而它是一个预测循环。语法很相似，但是while循环只需要一个表达式：

```c#
while(condition)
 statement(s);
```

与for循环不同，while循环最常用于重复一个语句或一个语句块。通常，while循环体中的一个语句会将一个布尔标志设置为false，从而触发循环的结束，如下例所示：

```c#
bool condition = false;
while (!condition)
{
 // This loop spins until the condition is true.
 DoSomeWork();
 condition = CheckCondition(); // assume CheckCondition() returns a bool
}
```



### do-while 循环

do-while循环是while循环的后测试版本。这意味着在循环主体执行之后再评估循环的测试条件。因此，do-while循环适用于循环体至少执行一次的情况：

```c#
bool condition;
do
{
 // This loop will at least execute once, even if the condition is false.
 MustBeCalledAtLeastOnce();
 condition = CheckCondition();
} while (condition);
```



### foreach 循环

foreach循环允许您遍历集合中的每个项。现在，不要担心一个集合到底是什么（它在第6章“数组”中有了充分的解释）；只是要理解它是一个表示对象列表的对象。严格来说，对象要算作集合，它必须支持一个名为IEnumble的接口。集合的示例包括c#数组、System.Collections命名空间和用户定义的集合类。arrayOfInts是int数组的一个集合类。

```c#
foreach (int temp in arrayOfInts)
{
 Console.WriteLine(temp);
}
```

在这里，foreach每次一步一步地通过数组中的一个元素。对于每个元素，它将元素的值放置在名为temp的int型变量中，然后执行循环的迭代。

这里是您可以使用类型推断的另一种情况。foreach循环将变成以下内容：

```c#
foreach (var temp in arrayOfInts)
{
 // ...
}
```

int将从临时项中进行推断，因为这是集合项的类型。

foreach需要注意的重要一点是，您不能更改集合中项的值（前面代码中的临时值），因此下面的代码将不会编译：

```c#
foreach (int temp in arrayOfInts)
{
 temp++;
 Console.WriteLine(temp);
}
```

如果需要遍历集合中的项并更改它们的值，则必须使用for循环。



### 退出循环

在循环中，您可以使用break语句停止迭代，或者结束当前迭代，并使用continue语句继续下一个迭代。使用return语句，您可以退出当前方法，因此也可以退出循环。



## 具有命名空间的组织

