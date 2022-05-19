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

对于int、short和long，如果应用程序是一个32位或64位的应用程序，则比特数和可用大小是独立的。这与用C-++定义的整数定义不同。c#9对于特定于平台的值有了新的关键字：nint和nuint（分别是本机整数和本机无符号整数）。在64位应用程序中，这些整数类型使用64位，而在32位应用程序中，仅使用32位。这些类型对于直接内存访问非常重要，这在第13章“托管和非托管内存”中涉及。

