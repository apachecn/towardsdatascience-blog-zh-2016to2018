# 我如何在非 Rails 环境中测试 Ruby APIs

> 原文：<https://towardsdatascience.com/how-i-test-ruby-apis-in-a-non-rails-environment-718aba24b189?source=collection_archive---------1----------------------->

我用 Ruby 编程已经快 5 年了，但是(喘气)不要用 [Ruby on Rails](http://rubyonrails.org/) 。尽管 [Sinatra](http://www.sinatrarb.com/) 是一个很好的框架，但它可能是 Rails 拥有 Ruby 社区 90%以上的份额之后才想到的。在 Rails 之外让一个 gem 工作并被很好地记录下来，几乎就像在苏联得到一双 Levi's。也就是说，通过一点点尝试和错误，你可以准备好任何宝石生产。

![](img/95b1c24f8220d037809c5f085ae665a0.png)

有时候，让一个专注于 rails 的 gem 在 Rails 之外工作需要更长的时间。例如，我的团队在弄清楚如何让 [ActiveRecord](https://github.com/rails/rails/tree/master/activerecord) 与多种模式一起工作之前，使用了将近一年的数据映射器。ActiveRecord 是 Rails 核心的一部分，除了自动重新连接失败之外，我们很喜欢它。我们的测试也有类似的问题，这导致我们放弃了测试覆盖率。直到我们的暑期实习生 Parker McGowan 整个夏天都在这个项目上，我们才最终得到了一个好的 Sinatra 测试套件。它看起来是这样的:

# 我们如何运行我们的测试

我们的团队使用非常传统的 web 开发设置。我们选择在前端(Angular)和后端(Ruby)之间进行硬分离。我们使用 Sinatra 框架运行微服务(小型 REST APIs ),并将数据存储在 MySQL 中。我们偶尔会用 Redis，Beanstalk 进行排队，联系 Twitter 之类的公共 API。为了测试，我们选择了 [MiniTest](https://github.com/seattlerb/minitest) 而不是 Rspec，因为[很像 Tenderlove](https://tenderlovemaking.com/2015/01/23/my-experience-with-minitest-and-rspec.html) ，我们更喜欢它的语法。它使用普通的老 ruby 代替 DSL，库现在在 core Ruby 中。

我们使用 SQLite 作为我们的测试数据库。不要使用 MySQL，因为您的测试会在每次运行时清除您的本地数据库。我们的数据库也遇到了一个有趣的问题。我的团队使用 SQL 已经有 10-15 年了，所以我们更喜欢手工创建表，而不是使用迁移来构建表。它给了我们更多的控制，但是超出了 Rails 的约定，使得用 ActiveRecord 测试变得很痛苦。我们手工创建我们的表，然后使用[独立迁移](https://github.com/thuss/standalone-migrations) gem 来生成迁移，然后我们可以使用它来动态生成我们的数据库以供测试。

*注意:如果您使用独立迁移，您将使用* ***。standalone _ migrations****文件而不是 Rakefile。看起来是这样的:*

```
# .standalone_migrationsconfig:
 database: config/development.yml
db:
 schema: test/db/schema.rb
```

我们用`bundle exec rake test`从命令行运行我们的测试

我们的每个测试都需要测试助手文件:

```
# /test/helper.rb**# Setup our test environment**
ENV[‘RACK_ENV’] = ‘test’
require ‘minitest/autorun’
require ‘mocha/mini_test’
require ‘rack/test’**# Include our application**
$LOAD_PATH.unshift ‘lib’
$LOAD_PATH.unshift ‘routes’
require File.expand_path ‘../../app.rb’, __FILE__ **# Sinatra App****# Setup database (and tear it down afterwards)**
test_db = SQLite3::Database.new(‘test/db/test.db’)Minitest.after_run do
 test_db.close
 File.delete(‘test/db/test.db’)
endActiveRecord::Base.establish_connection adapter: ‘sqlite3’, database: ‘test/db/test.db’
load ‘test/db/schema.rb’
```

这设置了我们的测试环境，包括我们的测试框架(rack 和 minitest)和 mocha 我们的模拟库。然后，在启动数据库和运行迁移之前，我们包括我们的应用程序特定的文件。由于 ActiveRecord 连接的运行方式，我们只运行一次迁移来构建我们的结构。为了保持测试的整洁，我们在每次测试后都清除数据库(截断表而不是删除它)。

# 试验

我们的自动化测试分为两类: [**单元**](https://en.wikipedia.org/wiki/Unit_testing) 和 [**集成**](https://en.wikipedia.org/wiki/Integration_testing) 。我为库和助手编写单元测试，为路由编写集成测试。单元测试范围很小，但是很全面。他们也跑得非常快。集成测试可以覆盖很多动作，有时会测试多个 API 调用。状态被大量修改，所以从一个干净的石板开始每个单独的测试是必要的。此外，您必须假设集成测试中的单元正在工作。稍后我们将讨论当我们测试特定于业务的代码时，如何使用模拟来忽略经过良好测试的库。

自动化测试基于一个简单的度量: [**断言**](http://docs.seattlerb.org/minitest/Minitest/Assertions.html) 。你要么通过你的断言，要么失败。尽管有大量的断言(和反驳)可用，我倾向于只使用这些:

```
**assert**        # is it truthy?**assert_nil**    # is it nil?**assert_equal**  # do the 2 parameters match?**assert_match**  # test string against a pattern
```

# 单元测试

那么应该如何选择用单元测试测试什么，用集成测试测试什么呢？我建议为副作用少、代码使用频繁的库编写单元测试。我认为为您的数据库模型编写它们是很忙的，因为您的 ORM 应该经过良好的测试。对于包装类(例如，包装对另一个 API 的调用的方法)，我也会忽略它们。话又说回来，我倾向于只写集成测试，除非一个库几乎没有副作用(错误不太可能发生在那些方法中看不到的代码中)并且经常被使用:

*   **人物计数器**——我的[公司](http://edgetheory.com)为社交媒体编写工具。我们必须在用户发送/安排消息之前验证文本的长度。
*   **Url shorter 的哈希生成器** —我们运行自己定制的 Url shorter。一个常见的函数是将整数(数据库表 ID)转换为 base 62 字符串，反之亦然。

以下是 Url Shortener 的一些测试:

```
require ‘./test/test_helper’class BaseConversionTest < MiniTest::Testdef setup
 [@logger](http://twitter.com/logger) = Logger.new(STDOUT)
 end# Encodedef test_convert_b10_to_b62_zero
 assert_equal(‘0’, Leadify::LinkShortener::Math.to_base(0, base=62))
 enddef test_convert_b10_to_b62_one_digit
 assert_equal(‘5’, Leadify::LinkShortener::Math.to_base(5, base=62))
 enddef test_convert_b10_to_b62_two_digit
 assert_equal(‘A’, Leadify::LinkShortener::Math.to_base(36, base=62))
 enddef test_convert_b10_to_b62_two_digit_b62
 assert_equal(‘10’, Leadify::LinkShortener::Math.to_base(62, base=62))
 enddef test_convert_b10_to_b62_two_char_b62
 assert_equal(‘1A’, Leadify::LinkShortener::Math.to_base(98, base=62))
 enddef test_convert_b10_to_b62_max_two_char_b62
 assert_equal(‘ZZ’, Leadify::LinkShortener::Math.to_base(3_843, base=62))
 end# ...
```

如你所见，这变得非常乏味。[代码覆盖率](https://en.wikipedia.org/wiki/Code_coverage)不够。您必须涵盖代码中可能遇到的所有变化。我们测试 0、5、36、62、98 和 3843。这给了我们一组广泛的数据，让我们可以专注于失败。我们知道，如果我们在新的基本系统中处理一位数、两位数、带进位的两位数，等等。

在为函数编写了这样的测试之后，我们仍然有可能错过一些东西。由于我们有编码和解码功能，我们可以使用我们的代码来测试自己。我们可以做一个[蒙特卡罗模拟](https://en.wikipedia.org/wiki/Monte_Carlo_method)，并使用函数`x = decode(encode(x))`测试一组随机输入。

```
# Reversibility Sampler
def test_monte_carlo_b10_to_b62_to_b10
 1000.times do |i|
  seed = rand(1_073_741_823) # Max 32 bit Fixnum
  b62 = Leadify::LinkShortener::Math.to_base(seed, base=62) 
  assert_equal(seed, Leadify::LinkShortener::Math.from_base(b62, base=62))
 end
end
```

但即使这样也不能囊括一切。你的编码器和解码器都可能损坏，但损坏的方式通过了`x = decode(encode(x))`测试。你的单元测试应该是健壮的，但是你不能抓住所有的东西。只要尽可能多的测试，当你看到一个失败时，添加一个测试。一旦你修改了你的代码，你就可以解决这个问题了。

如果你想看一套好的单元测试，看看[热门](https://github.com/rails/rails/blob/master/activerecord/test) [库](https://github.com/ohler55/oj/tree/master/test)。您还可以使用它们来理解文档记录不良的特性，或者找到有用的未记录的方法。通过阅读其他人的代码，你也可以学习编码风格。

# 集成测试

如果单元测试是树，那么集成测试就是森林。我们不再测试小而独特的组件。在我们的 Sinatra 集成测试中，我们通常测试单个 API 调用，然后测试一系列 API 调用。

**创建、读取、更新、删除(CRUD)** 路由大量使用我们的永久数据存储，我们使用 ActiveRecord 来访问它。正如我前面提到的，我们在生产中使用 MySQL，在测试中使用 sqlite。除了删除之外，我们所有的路由都返回 JSON 数据。因此，我们可以通过返回的 http 状态代码和输出 JSON 来验证 API 调用的正确性。这里有一个例子:

```
**def test_post_messages_with_message**
    post '/messages', text: '[@ash_ketchum_all](http://twitter.com/ash_ketchum_all), check out this pikachu I found digging through the dumpster behind the #Pokestop!'
    assert last_response.ok? response = Oj.load(last_response.body)
    assert(response.key?('message'))
    message = response['message']
    assert_equal '[@ash_ketchum_all](http://twitter.com/ash_ketchum_all), check out this pikachu I found   digging through the dumpster behind the #Pokestop!', message['text']
**end**
```

由于有了 [Rack::Test](https://github.com/brynary/rack-test) 库，我们可以进行这些 CRUD 调用。它提供了 http 方法，`get`、`post`、`put`、`patch`和`delete`。您可以用一种非常简单的方式来调用它们:

```
<http method> <route name>, <parameters>, <headers># Examples
get '/messages'
post '/messages', text: "Gotta catch 'em all"
post '/shares', { facebook_post_id: '1337' }, 'rack.session' => { visit_id: 1 }
```

这些 Rack::Test 方法允许我们导航我们的 API，MiniTest 允许我们断言应用程序的状态是我们所期望的。

我们的 CRUD 路线整合测试涵盖以下模式:

## **1)验证**

路由是否按照我们期望的方式处理不存在或无效的参数？每次验证失败时，它应该停止执行并返回一个错误代码，有时还会返回一条错误消息(例如，“name 是必填字段”)。

## 2)输出

首先，我们检查 http 状态是成功还是失败。如果它失败了，我们可以就此打住，我们必须找出它出错的原因。

接下来，我们检查 JSON 输出是什么。它是否返回了一个格式正确的对象？数据是我们预期的吗？如果我们有问题，具体是哪里的问题？

## 3)持久数据

当我们发布、上传或删除数据时，我们希望修改我们的数据。我们可以验证我们的 API 调用返回了正确的输出，但这并不意味着我们的应用程序永久地存储了数据。因为我们使用 ActiveRecord，所以我们可以在测试和生产中使用完全不同的数据库引擎。由于 ActiveRecord 经过了很好的测试，我们简单地假设，如果它在测试中有效，它将在生产中有效。为了验证永久存储中的数据是否被修改，我们只需要在 API 调用后检查我们的数据存储。

## 4)业务逻辑

API 中总是会有业务逻辑。如果它很小并且容易测试，那么我们可以把它移到库中，为它编写单元测试。那是首选。但是隔离这种行为通常很困难或者很费时间。对于这些情况，我们可以使用集成测试。例如，我们在新用户的注册过程中使用了大量的业务逻辑。我们首先创建一个登录名和一个帐户，然后为用户创建一个订阅，并将其连接到他们的 [Stripe](http://stripe.com) 订阅。然后，我们的一些用户在完成注册过程并登录我们的 web 应用程序之前授权他们的 twitter 帐户。

由于我们依赖于一些外部依赖(Stripe 和 Twitter)，我们不能通过在测试模式下调用我们的 API 来模拟这一点。我们必须启动一个真正的网络浏览器，通过模拟用户界面上的点击来进行端到端的测试。我们的 UI 超出了 API 测试的范围。此外，我们将依赖这些网站的可用性和速度。出于这些原因，模拟您的数据和模拟您的外部依赖性要好得多。

# 嘲弄

当我说 [mocks 时，我实际上是指 stubs](https://martinfowler.com/articles/mocksArentStubs.html) 。实际上，您可能会听到 *Mock* 既用于 Mock 又用于 stubs。FWIW，我们用[摩卡](https://github.com/freerange/mocha)(懂了吗？)宝石写我们的存根。存根通常用于覆盖类方法。他们的名字来源于他们的长相。下面是一个覆盖 Stripe 类方法的模拟示例:

```
**# Mock for a class method**
Stripe::Plan.stubs(:create).returns(
      stripe_uuid: 'vindaloo',
      name: 'Red Dwarf',
      subtitle: 'Spaceship',
      statement_descriptor: 'EDGETHEORY SB VIP',
      amount: 9999,
      currency: 'usd',
      interval: 'month',
      interval_count: 1,
      is_active: false
    )
```

模拟实例方法需要更多技巧。您必须首先创建一个模拟对象，然后用一个存根来扩展该模拟对象。下面是一个示例存根，它覆盖了他们的计划类的条带实例上的删除方法:

```
**# Mock for an instance method**
object = mock('object')
Stripe::Plan.stubs(:retrieve).returns(object)
object.stubs(:delete).returns(true)
```

这些模拟允许我们消除外部 api 的不可预测性和缓慢性。我们在 MiniTest 应用程序的 setup 块中运行这两个代码片段，这样当我们创建或删除一个计划时，我们可以得到一个可预测的结果。当我们创建一个计划时，我们总是返回相同的散列，我们也可以干净地删除一个。

# 例子

我在下面包含了一个完整的集成测试的例子，所以你可以看到一些相关的样板文件。我们包含了**测试助手**，并且必须使用 **app** 方法，这样 Rack::Test 就可以访问我们的 Sinatra app 并调用我们的内部 API routes。我还有**设置**(在测试用例运行之前)和**拆卸**(在测试用例完成之后)的方法。

```
require File.expand_path '../helper.rb', __FILE__class ShareTest < MiniTest::Test
  include Rack::Test::Methodsdef app
    EdgeTheory::Loyalty::ApplicationController # Require our Sinatra app
  end**# Called before each individual test case is run**
def setup
    @account  = EdgeTheory::DB::Loyalty::Account.create!(name: 'Your Cash Now', key: 'yourcashnow', is_active: true)
    @style    = EdgeTheory::DB::Loyalty::Style.create!(
      account_id: [@account](http://twitter.com/account).id,
      logo: '[http://rs278.pbsrc.com/albums/kk110/joker8851/PIKACHU.jpg~c200'](http://rs278.pbsrc.com/albums/kk110/joker8851/PIKACHU.jpg~c200')
    )
    @promotion = EdgeTheory::DB::Loyalty::Promotion.create!(account_id: @account.id)@reward = EdgeTheory::DB::Loyalty::Reward.create!(reward_type: 'instant',
                                                      name: 'Your Cash Now',
                                                      title: 'Fast cash, Money Now.',
                                                      instruction: 'Give us your social security number?')
    [@promotion](http://twitter.com/promotion).instant_reward_id = [@reward](http://twitter.com/reward).id
    [@promotion](http://twitter.com/promotion).save
    [@message](http://twitter.com/message) = EdgeTheory::DB::Loyalty::Message.create!(account_id: [@account](http://twitter.com/account).id, text: 'They gave me money')[@visit](http://twitter.com/visit) = EdgeTheory::DB::Loyalty::Visit.create!(promotion_id: [@promotion](http://twitter.com/promotion).id)
  end**# Called after each individual test case is completed**
def teardown
    ActiveRecord::Base.connection.tables.each do |table|
      ActiveRecord::Base.connection.execute("DELETE FROM #{table}")
      ActiveRecord::Base.connection.execute("DELETE FROM sqlite_sequence where name='#{table}'")
    end
    ActiveRecord::Base.connection.execute('VACUUM')
  enddef test_requires_visit
    err = assert_raises(EdgeTheory::Loyalty::SessionError) { post '/shares' }
    assert_match('Session has no visit_id', err.message)
  enddef test_share
    post '/shares', {}, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?
  enddef test_create_share
    post '/shares', {}, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?shares = EdgeTheory::DB::Loyalty::Share.all
    assert_equal(1, shares.length)
    assert_equal(1, shares[0].id)
    assert_equal(1, shares[0].visit_id)
  enddef test_create_share_with_facebook_post_id
    post '/shares', { facebook_post_id: '1337' }, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?shares = EdgeTheory::DB::Loyalty::Share.all
    assert_equal('1337', shares[0].facebook_post_id)
  enddef test_create_share_with_message
    post '/shares', { message_id: 1 }, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?shares = EdgeTheory::DB::Loyalty::Share.all
    assert_equal(1, shares[0].message_id)
  enddef test_create_share_with_invalid_message
    post '/shares', { message_id: 1_337 }, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?shares = EdgeTheory::DB::Loyalty::Share.all
    assert_equal(1, shares.length)
    assert_equal(1, shares[0].id)
    assert_equal(1_337, shares[0].message_id)
  enddef test_create_share_with_message_and_facebook_post_id
    post '/shares', { message_id: 1, facebook_post_id: '1337' }, 'rack.session' => { visit_id: [@visit](http://twitter.com/visit).id }
    assert last_response.ok?shares = EdgeTheory::DB::Loyalty::Share.all
    assert_equal(1, shares[0].message_id)
    assert_equal('1337', shares[0].facebook_post_id)
  end
end
```

# 进一步阅读

我希望你觉得这很有用。我不想写一个正式的教程，而是想把几个月的学习成果传递给大家，我绞尽脑汁，直到最终在 Ruby 中完成测试。

 [## 隔离测试 active record-Iain . nl

### 不加载 Rails 测试 ActiveRecord 并不困难。让我告诉你如何开始。

www.iain.nl](http://www.iain.nl/testing-activerecord-in-isolation)  [## 马特·西尔斯|迷你测试快速参考

### 给出了所有 MiniTest 断言的示例

www.mattsears.com](http://www.mattsears.com/articles/2011/12/10/minitest-quick-reference/) [](http://www.sinatrarb.com/testing.html) [## 使用 Rack 测试 Sinatra::Test

### 以下各节中的所有示例都假定正在使用，以尽可能做到通用。请参见…

www.sinatrarb.com](http://www.sinatrarb.com/testing.html) [](https://martinfowler.com/articles/mocksArentStubs.html) [## 模仿不是树桩

### 几年前，我在极限编程(XP)社区第一次接触到“模拟对象”这个术语。从那以后我就…

martinfowler.com](https://martinfowler.com/articles/mocksArentStubs.html)