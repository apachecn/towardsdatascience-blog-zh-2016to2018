# TDD 举例说明

> 原文：<https://towardsdatascience.com/tdd-explained-with-an-example-738d702f87e?source=collection_archive---------13----------------------->

## 建立一个 ASP.NET 维基来学习 TDD，这是一种使用小测试用例来构建更好软件的技术

我将尝试解释什么是 TDD，以及它如何在开发过程中提供帮助。有很多资源和书籍是这样做的，但我将尝试用一个简单的实际例子来介绍它。这更像是一个“哲学”概述，而不是你能在书中读到的严格定义。反正我也没想走纯理论的路子，而是更实践的方式，让你明白我们日常生活中真正需要的是什么。这种方法的纯粹主义支持者可能会发现这个解释有点不完整(抱歉…)，但我认为这足以开始学习和理解基础知识。

> 也许你不需要再读一本关于 TDD 的书，只要用清晰简单的文字理解它是什么就行了

这对初学者来说很好，可以激发兴趣，进行深入的探索，然后终生拥抱它。

![](img/84af0e779722ee4e17c8b644ec3c9427.png)

What is TDD? Test-driven development is a technique to build software using small test cases

# 什么是 TDD

从维基百科的定义开始:

> ***测试驱动开发****(****TDD****)是一种* [*软件开发过程，它依赖于一个非常短的开发周期的重复*](https://en.wikipedia.org/wiki/Software_development_process) *:需求被转化为非常具体的* [*测试用例*](https://en.wikipedia.org/wiki/Test_case) *，然后软件被改进以通过新的测试，只。这与允许添加未被证明满足需求的软件的软件开发相反。*

清楚了吗？TDD 的主要目的是创建一种策略，在这种策略中，测试将驱动开发过程，以便使编码更加高效、多产，减少回归。

先决条件是将一个大任务分解成更小的步骤，并使用单元测试进行开发。这允许您处理一小段代码，使它们工作，然后将许多工作部分集成在一起。

# TDD 的好处

将 TDD 引入您的编码体验将会达到一个转折点。以下是一些最重要的优势:

1.  关注真正重要的点:你会被要求分解问题，这将有助于你把注意力集中在最重要的事情上。
2.  处理更简单的任务:每次处理一个更小的任务可以简化故障排除，加快开发速度。你不会陷入这样的情况:你会写所有的代码，然后有些东西不工作，你不知道为什么。
3.  **简化集成**:当多个工作特性完成时，将所有特性组合在一起将是一件愉快而轻松的任务。在回归的情况下，你会提前知道代码的哪一部分是坏的。
4.  **免费测试**:一旦全部任务完成，大量的单元测试仍然存在，可以作为集成\单元测试来验证代码，避免回归。

# TDD 不是什么

TDD 是一种很好的方法，但不是:

*   测试的替换(单元测试、验收测试、UI 测试)
*   一天就能学会的东西
*   为你写代码的东西
*   一个从代码中驱除 bug 圣人

# TDD 生命周期

TDD 主要由三个步骤组成:

1.  编写单元测试(红色)。
2.  让它工作(绿色)。
3.  重构。

在示例中，您可以编写单元测试，使用其中的代码来实现该功能，直到它工作为止，然后在需要的地方重构这段代码。

## 步骤 1，2:让测试工作

```
public class StripTest
{
    [Fact]
    public static void StripHTml()
    {
        string test="<h1>test</h1>";
        string expected="test";
        string result=StripHTML(test);
        Assert.Equal(expected,result);
    }

    public static string StripHTML(string input)
    {
        return Regex.Replace(input, "<.*?>", String.Empty);
    }    
}
```

## 步骤 3:重构

```
public class StripTest
{
    [Fact]
    public static void StripHTml()
    {
        string test="<h1>test</h1>";
        string expected="test";
        string result=HtmlHelper.StripHTML(test);
        Assert.Equal(expected,result);
    }    
}

*//somewhere else*
public static class HtmlHelper
{
    public static string StripHTML(string input)
    {
        return Regex.Replace(input, "<.*?>", String.Empty);
    }
}
```

# 限制

在许多情况下，很难编写涵盖真实代码使用的单元测试。对于完全符合逻辑的过程来说，这很容易，但是当我们要涉及数据库或 UI 时，编写工作将会增加，并且在许多情况下，可能会超过好处。有一些最佳实践和框架对此有所帮助，但一般来说，并不是应用程序的所有部分都容易使用简单的单元测试来测试。

# 什么是 BDD？

BDD 是 TDD 的增强，它考虑了单元测试受限的情况。这个扩展将开发者作为一个单元测试，保持 BDD 背后的哲学。您仍然可以将复杂的任务分解成更小的任务，使用用户行为进行测试，并在纯后端任务上利用 TDD 的优势。

# TDD 先决条件

在团队中工作时，除了掌握所有相关技术的知识之外，所有的队友都必须了解并接受这一理念。

首先，您的代码必须得到强大的单元测试系统的支持:

*   。网，。NET Core:内置 Visual Studio 或者 Xunit(第二个是我个人，首选)
*   Java: JUnit 工作得非常好，我不需要寻找另一种解决方案
*   PHP: PHP 单元在所有情况下都为我工作

然后，重要且强制的是:拥有一个允许在测试期间模仿或重现正确行为的架构。我说的是一个 ORM，它可以在测试期间在内存或本地数据库上工作，但也可以使用服务或存储库模式。使用阿迪框架(内置。NET core，Autofac 或其他什么…)也有帮助。

最后但同样重要的是:一个做得好的构建过程，集成到一个持续的集成流程中，除了正确的配置之外，还要定义在集成期间在其上运行哪些单元测试是有意义的，以及哪些单元测试只是在本地运行。

# 这个例子

让我们试着在一个真实的例子中把我们学到的关于 TDD 的东西付诸实践。我想用这种方法创建一个维基。我指的是一个简单的 wiki，用户可以在这里登录、编写减价页面并发布。听起来很复杂？

> 那非常容易。多亏了 TDD 和管理小任务，我很快完成了所有的微特征，最后我把已经工作的部分组装起来。

首先，我会将“长期”任务分解成更小的后续活动。每个子部分将使用小单元测试来开发。我会把重点放在维基页面上。

# 步骤 1:实体到 DTO 的映射

从这里开始听起来不太好。实体到 DTO 的映射是一件非常原始的事情，很难抑制我们想要从最酷的部分开始的编码本能。无论如何，这是第一个，自动一致的功能。映射两个类只需要这两个类的定义，仅此而已。无论数据库连接，网络错误等等。我们只需要创建两个类(d to 和实体)，然后进行映射。最后，测试将是一段代码，它将检查实体中的字段是否被复制到 d to。轻松点。

让我们总结一下步骤:

1.  写实体。
2.  写维基页面 DTO。
3.  编写将实体映射到 DTO 的代码。

```
*// Database entity*
 public class WikiPageEntity
{
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public Guid Id { get; set; }

    public int Version { get; set; }
    public string Slug { get; set; }

    public string Body { get; set; }
    public string Title { get; set; }
}

*// DTO model in BLL*
namespace WikiCore.Lib.DTO
{
    public  class WikiPageDTO
    {
        public string Title { get; set; }
        public string BodyMarkDown { get; set; }
        public string BodyHtml { get; set; }
        public int Version { get; set; }
        public string Slug { get; set; }
    }
}

*// From unit test, code omitted for brevity*
public void EntityToDTO()
{
    WikiPageEntity source = new WikiPageEntity()
    {
        Title = "title",
        Slug = "titleslug",
        Version =1
    };

    var result = Mapper.Map<wikipagedto>(source);
    Assert.Equal("title", result.Title);
    Assert.Equal(1, result.Version);
}

*// From Mapping configuration, code omitted for brevity*
 public MappingProfile()
{
    CreateMap<wikipageentity, wikipagedto="">().ReverseMap();
}
```

# 步骤 2:降价到 HTML 转换

第二步是制作一个将`markdown`转换成 HTML 的方法。这将是一个非常简单的方法，它将接受一个 markdown 字符串，并检查它的转换是否与预期的 HTML 匹配。

```
*//Before refactoring public class MarkdownTest*
{
[Fact]
public void ConvertMarkDown()
{
    var options = new MarkdownOptions
    {
        AutoHyperlink = true,
        AutoNewLines = true,
        LinkEmails = true,
        QuoteSingleLine = true,
        StrictBoldItalic = true
    };

    Markdown mark = new Markdown(options);
    var testo = mark.Transform("#testo");
    Assert.Equal("<h1>testo</h1>", testo);
}
*// after refactoring ( method moved to helper )*
[Fact]
public void ConvertMarkDownHelper()
{
    Assert.Equal("<h1>testo</h1>", MarkdownHelper.ConvertToHtml("#testo"));
}

*// From markdown helper*
public static class MarkdownHelper
{
    static MarkdownOptions options;
    static Markdown converter;
    static MarkdownHelper()
    {
        options = new MarkdownOptions
        {
            AutoHyperlink = true,
            AutoNewLines = true,
            LinkEmails = true,
            QuoteSingleLine = true,
            StrictBoldItalic = true
        };

        converter = new Markdown(options);
    }

    public static string ConvertToHtml(string input)
    {
        Markdown mark = new Markdown(options);
        return mark.Transform(input);
    }
}
```

# 步骤 3:用降价增强映射

干得好！我们有从 markdown 生成 HTML 的方法和将实体翻译成 DTP 的映射器。下一步？把所有的放在一起！

下一段代码包含 HTML 字段计算的映射:

```
*// mapped profile changed*
public class MappingProfile : Profile
{

    public MappingProfile()
    {
        SlugHelper helper = new SlugHelper();
        CreateMap<wikipageentity, wikipagedto="">()
            .ForMember(dest => dest.BodyMarkDown, (expr) => expr.MapFrom<string>(x => x.Body))
            .ForMember(dest => dest.BodyHtml, 
            (expr) => expr.MapFrom<string>(x => MarkdownHelper.ConvertToHtml(x.Body)))
            .ReverseMap();

        CreateMap<wikipagebo,wikipageentity>()
            .ForMember(dest => dest.Body, (expr) => expr.MapFrom<string>(x => x.BodyMarkDown))
            .ForMember(dest => dest.Slug, 
                      (expr) => expr.MapFrom<string>(x => helper.GenerateSlug(x.Title)));
    }
}

*// From unit test, code omitted for brevity*
public void EntityToDTO()
{
    WikiPageEntity source = new WikiPageEntity()
    {
        Body = "# prova h1",
        Title = "title",
        Slug = "titleslug",
        Version =1
    };

    var result = Mapper.Map<wikipagedto>(source);
    Assert.Equal("title", result.Title);
    Assert.Equal(1, result.Version);
    Assert.Equal("<h1>prova h1</h1>", result.BodyHtml);
}
```

# 步骤 4:设置数据库迁移

另一步是整合数据库。要记住的一件重要的事情是，我们只需要测试一件事情…而数据库访问是一件复杂的事情。对数据库的第一个要求是结构。所以，检查的第一步是确保这个思想实体框架的迁移。

要执行的步骤:

1.  运行`Add-Migration`脚本。
2.  创建一个在内存中工作的单元测试来测试它。

```
[Fact]
public void MigrateInMemory()
{

    var optionsBuilder = new DbContextOptionsBuilder<DatabaseContext>();
    optionsBuilder.UseInMemoryDatabase();

    using (var db = new DatabaseContext(optionsBuilder.Options))
    {
        db.Database.Migrate();
    }
    *// No error assert migration was OK*
}
```

# 步骤 5:实体积垢

在我们设置好迁移之后，我们可以假设数据结构一切正常。让我们从证明 CRUD 特性的单元测试开始。

步骤:

1.  编写一个 CRUD 测试。
2.  测试一下。

```
[Fact]
public void CrudInMemory()
{
    var optionsBuilder = new DbContextOptionsBuilder<DatabaseContext>();
    optionsBuilder.UseInMemoryDatabase();

    using (var db = new DatabaseContext(optionsBuilder.Options))
    {
        db.Database.Migrate(); 

        db.WikiPages.Add(new Lib.DAL.Model.WikiPageEntity()
        {
            Title = "title",
            Body = "#h1",
            Slug = "slug"

        });

        db.SaveChanges();

        var count=db.WikiPages.Where(x => x.Slug == "slug").Count();

        Assert.Equal(1, count);
        *// update, delete steps omitted for brevity*
    }
}
```

# 步骤 6:测试服务

在我们的架构中，服务层将提供业务逻辑的抽象。在这个简单的例子中，我们的服务将包装插入或更新特性，在保存后返回一个 DTO。

这个单元测试的步骤:

1.  用业务逻辑创建服务。
2.  测试一下

```
[Fact]
public void TestSave()
{
    var optionsBuilder = new DbContextOptionsBuilder<DatabaseContext>();
    optionsBuilder.UseInMemoryDatabase();

    using (var db = new DatabaseContext(optionsBuilder.Options))
    {
        db.Database.Migrate();
        db.SaveChanges();

        *//this recreate same behaviour of asp.net MVC usage*
        DatabaseWikiPageService service = new DatabaseWikiPageService(db, Mapper.Instance);
        service.Save(new Lib.BLL.BO.WikiPageBO()
        {
            BodyMarkDown="#h1",
            Title="prova prova"
        });

        var item = service.GetPage("prova-prova");
        Assert.NotNull(item);
    }
}
```

# 步骤 7:在用户界面上继续

一旦使用单元测试测试 UI 变得复杂，我就从纯粹的 TDD 方法切换到一个更有弹性的测试版本，并参与到这个过程中。这有助于将所有工作分成多个步骤来完成 UI。因此，我没有编写所有代码然后测试它，而是将问题分解成多个子活动，然后逐一测试:

**编辑**

1.  准备表单，并测试它。
2.  准备模型，测试从表单提交的内容填充后端模型。
3.  集成服务以保存数据，测试数据。

**视图**

1.  准备模型，传递给视图，测试它。
2.  将模型与服务集成，以获得真实数据。测试一下。

**列表**

1.  准备视图模型，传递假数据到 UI，测试它。
2.  集成服务，测试它。

每个微特征都可以快速实现并易于测试。这将促进完整的实现。

# 结论

> TDD 是一种驱动测试支持的开发过程的方法。

这有助于在许多方面编码，但要求所有的队友都有一些基础知识。一旦完成了这一步，您将处理一个更简单的任务和许多可以重用的测试。

> 这个过程将有助于避免回归，并更快地达到目标，如果在开发的同时努力编写单元测试也是如此。

此外，如果您的应用程序由于复杂性而难以测试，您可以保持同样的理念执行一些手动步骤。

觉得这篇文章有用？在 Medium 上关注我([丹尼尔·丰塔尼](https://medium.com/@daniele.fontani))，看看我下面最受欢迎的文章！请👏这篇文章分享一下吧！

*   [Docker 到底是什么？](https://medium.com/swlh/what-is-docker-28bd2b618eee)
*   【Kubernetes 到底是什么？
*   [如何使用 Kubernetes 部署 web 应用程序](https://medium.com/swlh/how-to-deploy-an-asp-net-application-with-kubernetes-3c00c5fa1c6e)

# 资源

*   完整的 git 源代码[https://github.com/zeppaman/wiki.asp.net.core](https://github.com/zeppaman/wiki.asp.net.core)
*   原文发表于 2018 年 11 月 17 日[www.codeproject.com](https://www.codeproject.com/Articles/1267361/Build-an-ASP-NET-Wiki-to-Explain-TDD)。
*   关于 TDD[https://en.wikipedia.org/wiki/Test-driven_development](https://en.wikipedia.org/wiki/Test-driven_development)维基百科告诉了什么