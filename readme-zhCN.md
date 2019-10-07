<img src="/assets/jtbp-header-blue.png" width="1920px"/>

<br/>

# 👇 Why this guide can take your testing skills to the next level 

<br/>

## 📗 45+ best practices: Super-comprehensive and exhaustive
This is a guide for JavaScript & Node.js reliability from A-Z. It summarizes and curates for you dozens of the best blog posts, books and tools the market has to offer


## 🚢 Advanced: Goes 10,000 miles beyond the basics
Hop into a journey that travels way beyond the basics into advanced topics like testing in production, mutation testing, property-based testing and many other strategic & professional tools. Should you read every word in this guide your testing skills are likely to go way above the average


## 🌐 Full-stack: front, backend, CI, anything
Start by understanding the ubiquitous testing practices that are the foundation for any application tier. Then, delve into your area of choice: frontend/UI, backend, CI or maybe all of them?

<br/>

### Written By Yoni Goldberg
* A JavaScript & Node.js consultant
* 👨‍🏫 [My testing workshop](https://www.testjavascript.com) -  learn about [my workshops](https://www.testjavascript.com) in Europe & US
* [Follow me on Twitter ](https://twitter.com/goldbergyoni/)
* Come hear me speak at [LA](https://js.la/), [Verona](https://2019.nodejsday.it/), [Kharkiv](https://kharkivjs.org/), [free webinar](https://zoom.us/webinar/register/1015657064375/WN_Lzvnuv4oQJOYey2jXNqX6A). Future events TBD
* [My JavaScript Quality newsletter](https://testjavascript.com/newsletter/) - insights and content only on strategic matters


<br/><br/>

## `内容`

#### [`第0章：黄金准则`](#section-0️⃣-the-golden-rule)

一条给所有人的建议（1小节）

#### [`第1章：测试剖析`](#section-1-the-test-anatomy-1)

基础——构建整洁的测试 (12小节)

#### [`第2章：后端`](#section-2️⃣-backend-testing)

高效地编写后端和微服务测试 (8 小节)

#### [`第3章：前端`](#section-3️⃣-frontend-testing)

为web UI编写测试，包括组件和E2E测试(11 小节)

#### [`第4章：衡量测试的有效性`](#section-4️⃣-measuring-test-effectiveness)

衡量测试质量 (4 小节)

#### [`第5章：持续集成`](#section-5️⃣-ci-and-other-quality-measures)

JS世界中的CI指南 (9小节)

<br/><br/>


# 第0️⃣章: 黄金准则

<br/>

## ⚪️ 0. 黄金准则: 设计精炼的测试

:white_check_mark: **最佳实践:** 
测试代码和生产代码不同 - 它们应该被设计得简洁、非抽象、扁平、精炼。人们应该看一眼测试代码就能马上明白意图。

我们的脑子已经塞满了生产代码，没有脑容量去塞下更多复杂的东西。复杂的测试代码会拖慢团队的速度，这就和我们写测试的初衷相违背了，这也是为什么很多团队放弃写测试。

测试应该像一个友好的“助手”，我们可以愉快地和它共事，并且只需要投入较小的成本就能给我们带来巨大的回报。科学告诉我们人脑有两个系统：系统1用于轻松的活动，比如在空路上开车；系统2用于复杂和有意识的操作，比如解数学题。我们应该以系统1为目标设计测试，在查看测试代码时，它们应该像修改HTML文档一样简单，而不是像计算2 x (17 × 24)。

这可以通过选择“成本效益”和“投资回报率”高的技术、工具和测试目标来实现。仅对有需要的地方进行测试，努力保持灵活性，有时甚至可以放弃一些测试和可靠性来换取灵活性和简单性。

![alt text](/assets/headspace.png "We have no head room for additional complexity")

下文的大多数建议都是基于上述准则。

### 准备好了吗?

<br/><br/>

# 第1章: 测试剖析

<br/>

## ⚪ ️ 1.1 每个测试名中包含3部分

:white_check_mark: **最佳实践：**一份测试报告应该能让不一定熟悉代码的人（测试人员、正在部署的开发运维工程师以及两年后的你自己）也能知道当前版本的应用是否符合要求。如果测试在需求级别进行，并且包括3个部分，则可以达到最佳效果：

(1) 正在测试什么? 比如, `ProductsService.addNewProduct` 方法

(2) 在什么情况下? 比如, 没有向方法传递任何价格参数

(3) 预期结果是什么? 比如, 新产品不被批准

<br/>


❌ **否则：** 部署失败, 一个叫 “Add product” 的测试失败。 这种提示能告诉你实际上是什么故障码?

<br/>

**👇 提示：** 点击展开

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：由3个部分组成的测试名称

![](https://img.shields.io/badge/🔨%20Example%20using%20Mocha-blue.svg
 "Using Mocha to illustrate the idea")

```javascript
//1. 被测单元
describe('产品服务', function() {
  describe('添加新产品', function() {
    //2. 场景 和 3. 期望
    it('没有指定价格时，产品状态是等待批准', ()=> {
      const newProduct = new ProductService().add(...);
      expect(newProduct.status).to.equal('pendingApproval');
    });
  });
});

```
<br/>

### :clap: 正确示范：由3个部分组成的测试名称
![alt text](/assets/bp-1-3-parts.jpeg "A test name that constitutes 3 parts")

</details>

<br/><br/>

## ⚪ ️ 1.2 采用AAA模式来构建测试

:white_check_mark: **最佳实践：**用完全分离的布置（Arrange）、操作（Act）和断言（Assert）三个部分来构建测试代码. 遵循这种结构可以保证读者在理解测试计划时不会花费任何大脑CPU：

第1个A - 布置（Arrange）：模拟测试目标环境的所有设置代码。这可能包括在测试构造函数中实例化单元、添加数据库记录、对对象进行模拟/打桩操作以及任何其他准备代码。

第2个 A - 操作（Act）：执行被测单元。通常一行代码。

第3个 A - 断言（Assert）：保证接收到的值满足预期。通常一行代码。

<br/>


❌ **否则：**需要花费很长时间理解测试代码

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：采用AAA模式来构建的测试

![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest") ![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

```javascript
describe('顾客分类器', () => {
    test('当顾客消费了超过500$时, 应该被分类为高级顾客', () => {
        //布置
        const customerToClassify = {spent:505, joined: new Date(), id:1}
        const DBStub = sinon.stub(dataAccess, "getCustomer")
            .reply({id:1, classification: 'regular'});

        //操作
        const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);

        //断言
        expect(receivedClassification).toMatch('premium');
    });
});
```

<br/>

### :thumbsdown: 反例：没有分离, 一坨代码, 更难理解

```javascript
test('应该被分类为高级顾客', () => {
        const customerToClassify = {spent:505, joined: new Date(), id:1}
        const DBStub = sinon.stub(dataAccess, "getCustomer")
            .reply({id:1, classification: 'regular'});
        const receivedClassification = customerClassifier.classifyCustomer(customerToClassify);
        expect(receivedClassification).toMatch('premium');
    });
```


</details>



<br/><br/>




## ⚪ ️1.3 使用产品语言来描述期望：使用BDD风格的断言

:white_check_mark: **最佳实践：**使用声明式风格来编写测试代码能让读者马上理解意图。 当你写一段带条件逻辑的命令式代码时，读者需要花费更多的脑力去思考。所以应该用类人语言来编写测试期望，使用 `expect` 或`should`来声明BDD（行为驱动开发）风格代码，而非使用自定义代码。如果 Chai和Jest没有包含你需要的断言，并且这个断言是经常复用的，可以考虑[扩展Jest matcher (Jest)](https://jestjs.io/docs/en/expect#expectextendmatchers) 或者写一个[Chai插件](https://www.chaijs.com/guide/plugins/)。
<br/>


❌ **否则：** 团队将编写更少的测试并用`.skip()`修饰某些烦人的测试

<details><summary>✏ <b>Code Examples</b></summary><br/>

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Mocha & Chai") ![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest")

  ### :thumbsdown: 反例：读者必须浏览不那么短的命令式代码，以获知测试代码的含义

```javascript
test("当请求管理员时, 保证结果中只有被请求的管理员" , () => {
    //假设我们在这里已经添加了两个管理员"admin1"、"admin2"，以及（用户）"user1"
    const allAdmins = getUsers({adminOnly:true});

    const admin1Found, adming2Found = false;

    allAdmins.forEach(aSingleUser => {
        if(aSingleUser === "user1"){
            assert.notEqual(aSingleUser, "user1", "找到了一位用户，并非管理员");
        }
        if(aSingleUser==="admin1"){
            admin1Found = true;
        }
        if(aSingleUser==="admin2"){
            admin2Found = true;
        }
    });

    if(!admin1Found || !admin2Found ){
        throw new Error("并非所有（请求）的管理员都被返回");
    }
});

```
<br/>

### :clap: 正确示范：浏览下面声明式的测试代码轻而易举


```javascript
it("当请求管理员时, 保证结果中只有被请求的管理员" , () => {
    //假设我们已经添加了两个管理员
    const allAdmins = getUsers({adminOnly:true});

    expect(allAdmins).to.include.ordered.members(["admin1" , "admin2"])
  .but.not.include.ordered.members(["user1"]);
});

```

</details>


<br/><br/>


## ⚪ ️  1.4 坚持黑盒测试：只测试公共方法

:white_check_mark: **最佳实践：** 测试内部实现会带来几乎无意义的巨大成本。 如果你的代码/API提供了正确的结果，那么你是否值得继续花费3个小时去测试它的内部是如何运作的，然后维护这些脆弱的测试？ 当检查公共方法时, 内部私有的实现也会被隐式地测试，并且只有出现某个问题（比如错误的输出）时测试才会中断。这种方法也被成为行为测试。另一方面，如果你测试内部实现（白盒测试），你关注的重点就从结果转移到具体的细节并且你的测试可能会因为很小的代码重构而中断（尽管结果还是对的）——这会显著增加维护的负担。
<br/>


❌ **否则:** 你的测试就像 [狼来了](https://en.wikipedia.org/wiki/The_Boy_Who_Cried_Wolf): 经常出现假阳性错误 （例如一个测试仅仅因为某个私有变量名被改了而没跑过）。不出所料的话，人们会因此渐渐忽略CI抛出来的告警直到某天一个真正的bug也被忽略了……

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例: 一个测试用例在没有充分理由的情况下测试内部实现
![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Mocha & Chai")
```javascript
class ProductService{
  //这个方法只在内部使用
  //改变方法名会让测试失败
  calculateVAT(priceWithoutVAT){
    return {finalPrice: priceWithoutVAT * 1.2};
    //改变以上结果的格式或键名会使测试失败
  }
  //公共方法
  getPrice(productId){
    const desiredProduct= DB.getProduct(productId);
    finalPrice = this.calculateVATAdd(desiredProduct.price).finalPrice;
  }
}


it("白盒测试: 当内部方法得到0增值税时, 返回0作为结果", async () => {
    //没有需求要让用户去算增值税, 只展示最终价格即可。然而我们错误地坚持在这里测试类的内部实现。
    expect(new ProductService().calculateVATAdd(0).finalPrice).to.equal(0);
});

```

</details>




<br/><br/>

## ⚪ ️ ️1.5 选择正确的测试替身: 避免mocks，使用stubs 和spies

:white_check_mark: **最佳实践:**  测试替身（Test doubles）是一种“必要的邪恶（necessary evil）”，因为它们和程序内部耦合在一起，但有些提供了巨大的价值 (<a href="https://martinfowler.com/articles/mocksArentStubs.html" data-href="https://martinfowler.com/articles/mocksArentStubs.html" class="markup--anchor markup--p-anchor" rel="noopener nofollow" target="_blank">[在此阅读有关测试替身的知识: mocks vs stubs vs spies](https://martinfowler.com/articles/mocksArentStubs.html)</a>)。

在使用测试替身之前，请问自己一个非常简单的问题：我是否用它来测试在需求文档中出现或可能出现的功能？如果没有，那有点白盒测试的味道。

例如，如果你想测试程序在支付服务关闭时的行为是否合理，你可以stub支付服务并触发一些“无响应”返回，以确保被测单元返回正确的值。这将检查特定场景下的应用程序行为/响应/结果。你还可以使用一个spy来断言当服务关闭时有发送电子邮件——这也是一种很可能会出现在需求文档中的行为检查（“如果无法保存付款，则发送电子邮件”）。另一方面，如果你是mock支付服务并检查它是否被正确的javascript类型所调用，那么你的测试就是将重点放在了与应用程序功能无关且可能经常更改的内部实现上。
<br/>


❌ **否则:** 任何代码重构都需要搜索代码中的所有mocks并相应地更新。测试成了一种负担。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例: Mocks专注于内部实现
![](https://img.shields.io/badge/🔧%20Example%20using%20Sinon-blue.svg
 "Examples with Mocha & Chai")
```javascript
it("当一个有效商品要被删除时, 保证用正确的产品和配置只调用一次数据访问DAL", async () => {
    //假设我们已经添加了一项产品
    const dataAccessMock = sinon.mock(DAL);
    //BAD: 这里我们的主要目标变成了测试内部实现，而不仅仅让它成为副作用
    dataAccessMock.expects("deleteProduct").once().withArgs(DBConfig, theProductWeJustAdded, true, false);
    new ProductService().deletePrice(theProductWeJustAdded);
    dataAccessMock.verify();
});
```
<br/>

### :clap:正确示范: spies专注于测试需求，但副作用是无可避免的会触碰到内部实现

```javascript
it("当一个有效商品要被删除时, 保证发送一封邮件", async () => {
    //假设我们已经添加了一项产品
    const spy = sinon.spy(Emailer.prototype, "sendEmail");
    new ProductService().deletePrice(theProductWeJustAdded);
    //OK: 我们涉及了内部实现? 是的, 但只作为测试需求（发送邮件）的一种副作用 
});
```

</details>



<br/><br/>

## ⚪ ️1.6 不要写“foo”，使用真实的输入数据

:white_check_mark: **最佳实践：**  通常生产环境的bugs是在一些很特定和出乎意料的输入下被发现的——所以测试的输入越真实，越容易提前发现bugs。使用像[Faker](https://www.npmjs.com/package/faker)那样专用的库来生成接近生产环境的伪数据。例如，这些库可以生成真实的电话号码、用户名、信用卡、公司名，甚至是“lorem ipsum（译者注：一篇常用于排版设计的拉丁文文章）”文本。你还可以创建一些测试（在单元测试之上，而不是替代）去随机化这些伪数据来扩展你的测试单元，或者甚至从生产环境中导入真实数据。想更上一层楼？请看下一节（基于属性的测试）。
<br/>


❌ **否则：** 当你用诸如“foo”之类作为输入时，开发环境中的测试很可能错误地显示通过，但在生产环境中却出现了bug——比如黑客输入了类似“@3e2ddsf . ##’ 1 fdsfds . fds432 AAAA”的特殊字符。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例： 一个由于没有使用真实数据而得以通过的测试套件

![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest")


```javascript
const addProduct = (name, price) =>{
  const productNameRegexNoSpace = /^\S*$/;//不允许空白字符

  if(!productNameRegexNoSpace.test(name))
    return false;//这个分支由于输入的局限不会被触及

    //一些代码逻辑
    return true;
};

test("错误的: 当用有效的属性添加产品时, 获得成功的确认消息", async () => {
    //用于所有测试的“Foo”字符串永远不会触发false结果
    const addProductResult = addProduct("Foo", 5);
    expect(addProductResult).toBe(true);
    //假阳性: 操作会成功是因为我们从来没有用长的、包含空格的产品名去测试
});

```
<br/>

### :clap:正确示范：随机化真实输入
```javascript
it("更好的: 当添加新的有效产品时，获得成功的确认消息", async () => {
    const addProductResult = addProduct(faker.commerce.productName(), faker.random.number());
    //生成随机输入: {'Sleek Cotton Computer',  85481}
    expect(addProductResult).to.be.true;
    //测试失败, （因为）随机输入触发了一些我们意料之外的代码分支。
    //我们提前发现了一个bug！
});
```

</details>




<br/><br/>

## ⚪ ️ 1.7 使用基于属性的测试来测试多个输入组合

:white_check_mark: **最佳实践：** 通常我们会为每个测试选择一些输入样本。尽管输入数据的格式做到了近似真实数据（见上一节“不要写foo”)，我们也只能覆盖一部分的输入组合（`method('', true, 1)`， `method('string', false, 0)`）。然而在生产环境，一个被5个参数调用的API可以以数千种参数排列的形式被调用，而其中一种可能会使我们的程序挂掉([见Fuzz测试](https://en.wikipedia.org/wiki/Fuzzing))。是否可以编写一个单独的测试，能自动发送1000种不同的输入排列，并捕获我们的代码未能返回正确响应的那种输入？基于属性的测试（Property-based testing）就是可以实现这种需求的技术：通过将所有可能的输入组合发送到被测单元，可以增加发现bugs的机会。比如，给定一个方法` addNewProduct(id, name, isDiscount)` ——支持库将使用很多（数字，字符串，布尔值的）组合来调用此方法，比如`(1, “iPhone”, false)`，`(2, “Galaxy”, true)`。你可以在自己喜欢的测试运行程序（Mocha, Jest, 等等）中使用诸如[js-verify](https://github.com/jsverify/jsverify)或[testcheck](https://github.com/leebyron/testcheck-js)（文档好得多）等库。更新：Nicolas Dubien在评论中推荐[checkout fast-check](https://github.com/dubzzz/fast-check#readme)，看起来提供了一些额外的功能，作者也在积极维护。
<br/>


❌ **否则：** 你会无意识地选择那些只覆盖了工作良好的代码的测试输入。不幸的是，这会降低测试暴露bugs的效率。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  正确示范：用“mocha-testcheck”来测试多种输入排列

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

```javascript
require('mocha-testcheck').install();
const {expect} = require('chai');

describe('产品服务', () => {
  describe('增加新产品', () => {
    //这里会用不同的随机属性跑100次
    check.it('使用随机属性添加产品, 总是成功',
      gen.int, gen.string, (id, name) => {
        expect(addNewProduct(id, name).status).to.equal('approved');
      });
  })
});

```

</details>




<br/><br/>

## ⚪ ️ 1.8 如果需要，只使用简短且内联的快照

:white_check_mark: **最佳实践：** 当需要做[快照测试](https://jestjs.io/docs/en/snapshot-testing)的时候，只使用简短而专注（focused）的（即3-7行）、内联到测试文件中的快照（[内联快照](https://jestjs.io/docs/en/snapshot-testing#inline-snapshots)），而不要（把快照）放在外部文件中。遵守这条方针将确保你的测试保持自解释性且不那么脆弱。

另一方面，“经典快照”教程和工具鼓励在一些外部介质上存储大文件（例如组件渲染标记、API JSON结果），并确保每次测试运行时将接收到的结果与保存的版本进行比较。这种做法，举个例子，可以将我们的测试与一个1000行、具有3000个数据值（的外部文档）隐式地耦合，这些数据值是测试编写者从未阅读和推理过的。为什么这是不对的？因为这样做，测试失败的原因（可以）有1000个——只需要更改一行就足以使快照失效，并且这种情况很可能经常发生。有多经常？当每个空格、注释或细微的CSS/HTML发生改变的时候。不仅如此，测试名称还不会给出测试失败的线索，因为它只检查了那1000行没有改变，并且它鼓励测试编写者接受一个他无法检查和验证的长文档作为期望值。所有这些都是含糊不清、急于求成的测试的症状，这些测试没有集中精力，想要实现的东西太多。

值得注意的是，在少数情况下冗长的、存储在外部的快照是可以接受的——当断言的是架构而不是数据时（提取值并关注字段），或者当这个外部文档很少会更改时。
<br/>

❌ **否则：** UI测试失败。代码看起来没错，屏幕完美地渲染了每一个像素，那是为啥？因为你的快照测试检查到了原始（参考）文档和当前渲染文档有一处差异——多了一个空格……

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例：将我们的测试与2000行看不见的代码耦合

![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest")

```javascript
it('TestJavaScript.com渲染正确', ()  => {

//布置

//操作
const receivedPage = renderer
.create(  <DisplayPage page  =  "http://www.testjavascript.com"  > Test JavaScript < /DisplayPage>)
.toJSON();

//断言
expect(receivedPage).toMatchSnapshot();
//我们现在隐式地维护了一个2000行的长文档
//每一个额外的换行符或注释都将破坏此测试

});
```
<br/>

### :clap: 正确示范：期望值是可见且专注的
```javascript
it('当访问TestJavaScript.com主页时, 会展示一个菜单', () => {
//布置

//操作
receivedPage tree = renderer
.create(  <DisplayPage page  =  "http://www.testjavascript.com"  > Test JavaScript < /DisplayPage>)
.toJSON();

//断言

const menu = receivedPage.content.menu;
expect(menu).toMatchInlineSnapshot(`
<ul>
<li>Home</li>
<li> About </li>
<li> Contact </li>
</ul>
`);
});
```

</details>


<br/><br/>

## ⚪ ️1.9 避免使用全局的test fixtures和seeds，为每个测试添加数据

:white_check_mark: **最佳实践：**按照黄金准则（第0节），每个测试都应该添加自己的一组数据并只对它进行操作，以防止耦合并轻松推理测试流。但实际上，为了提高性能，在运行测试前为数据库播种数据（[也称为“测试夹具（test fixture）”](https://en.wikipedia.org/wiki/test_fixture)）的测试人员经常会违反这一点。虽然性能的确是一个合理的考量——它可以被减轻（参见“组件测试”一节），但是，测试复杂性是一个非常痛苦的悲伤，应该在大多数时候优先于其他考量。实际上，（应该）让每个测试用例显式地添加它需要的数据库记录，并且只对这些记录执行操作。如果性能成为一个关键问题——一个可能的折衷方案是只播种唯一一组不改变数据的测试（例如查询）。
<br/>


❌ **否则：** 有几个测试没通过，部署中断了，我们的团队现在要花费宝贵的时间查bugs。让我们来研究一哈，oh no，两个测试好像在更改同一个种子数据。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例：测试不是独立的，依赖一些全局钩子来提供全局数据

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

```javascript
before(() => {
  //添加站点和管理员数据到数据库。数据在哪儿？外部一些json文件或迁移框架中
  await DB.AddSeedDataFromJson('seed.json');
});
it("当更新站名时，获取成功的确认消息", async () => {
  //我知道"portal"这个站名存在——我在（数据库）种子文件里看见它了
  const siteToUpdate = await SiteService.getSiteByName("Portal");
  const updateNameResult = await SiteService.changeName(siteToUpdate, "newName");
  expect(updateNameResult).to.be(true);
});
it("当用站名请求（网站）时, 获得正确的网站", async () => {
  //我知道"portal"这个站名存在——我在（数据库）种子文件里看见它了
  const siteToCheck = await SiteService.getSiteByName("Portal");
  expect(siteToCheck.name).to.be.equal("Portal"); //失败! 前一个测试改变了站名 :[
});

```
<br/>

### :clap: 正确示范：每个测试都只对自己的数据集进行操作

```javascript
it("当更新站名时，获取成功的确认消息", async () => {
  //测试被添加了全新的数据记录，并且只对这些记录进行操作
  const siteUnderTest = await SiteService.addSite({
    name: "siteForUpdateTest"
  });
  
  const updateNameResult = await SiteService.changeName(siteUnderTest, "newName");
  
  expect(updateNameResult).to.be(true);
});

```

</details>


<br/>

## ⚪ ️ 1.10 不要catch错误，expect它们
:white_check_mark: **最佳实践：**当尝试断言某些输入会触发错误时，使用`try-catch-finally`并断言进入了`catch`区块的做法看起来好像没错。（但）结果是一个笨拙而冗长的测试用例（下面的示例），它隐藏了简单的测试意图和结果期望。

一个更优雅的选择是使用一行专用的`Chai`断言：`expect.to.throw`（或使用`Jest`：`expect.toThrow()`）。另外，还必须确保（抛出的）异常（exception）包含一个提示错误类型的属性，否则如果只给出一般性错误，程序除了向用户展示一条令人失望的（错误）消息外也做不了更多。
<br/>

❌ **否则：**很难从测试报告（如CI报告）中推断到底出了什么错

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例：一个试图用try-catch来断言错误存在的长测试用例

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

```javascript
it("没有产品名时，抛出400错误", async() => {
let errorWeExceptFor = null;
try {
  const result = await addNewProduct({name:'nest'});}
catch (error) {
  expect(error.code).to.equal('InvalidInput');
  errorWeExceptFor = error;
}
expect(errorWeExceptFor).not.to.be.null;
//如果这个断言失败了，那么测试结果/报告只会展示某些值是null，不会提及有一个丢失的Exception
});

```
<br/>

### :clap: 正确示范：一个人类可读的、易理解的期望，甚至QA或技术PM也能理解

```javascript
it.only("没有产品名时，抛出400错误", async() => {
  expect(addNewProduct)).to.eventually.throw(AppError).with.property('code', "InvalidInput");
});

```

</details>




<br/><br/>

## ⚪ ️ 1.11 给你的测试打tag

:white_check_mark: **最佳实践：**不同测试必须在不同场景下运行：快速冒烟（quick smoke），无IO（IO-less），测试应该在开发人员保存或提交一个文件时运行，完整的端到端测试通常在提交新的请求时运行，等等。这可以通过使用关键字（如#cold #api #sanity）给测试打tag来实现，这样你就可以使用测试工具进行grep并调用所需的子集。例如，这就是使用Mocha时只调用sanity测试组的方法：`mocha — grep 'sanity'`。
<br/>


❌ **否则：** 要运行所有的测试，（可能）包括要执行几十个数据库查询的测试，每次开发人员要做一个小更改都会非常慢，让开发人员远离运行测试。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范: 将测试标记为“#cold-test”允许测试者只执行快速测试（Cold 意为不执行IO的快速测试，即使开发人员在快速键入也能频繁地执行）

![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest")
```javascript
//这个测试很迅速(不涉及数据库) 并且我们正确地标记了它
//现在用户/CI可以频繁地运行它
describe('下单服务', function() {
  describe('增减新订单 #cold-test #sanity', function() {
    test('场景——没有提供货币。Expectation——使用默认货币 #sanity', function() {
      //这里有代码逻辑
    });
  });
});


```

</details>




<br/><br/>

## ⚪ ️1.12 其他通用的好的测试准则
:white_check_mark: **最佳实践：**  本文主要讨论与Node JS相关的或者至少可以用Node JS举例说明的测试建议。然而，这一节总结了一些广为人知的与Node无关的tips。

学习和实践[测试驱动开发（TDD）原则](https://www.sm-cloud.com/book-review-test-driven-development-by-example-a-tldr/) —— 它们对很多人来说非常有价值，但如果不符合你的风格那就算了。考虑在写代码前以[红-绿-重构样式（red-green-refactor style）](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)编写测试代码，保证每个测试都只检查一件事，当你发现了一个bug——在修复之前，请编写一个测试用于将来检测这个bug，让每个测试在通过之前至少失败一次，通过编写一段满足测试的快速简单的代码来启动一个模块——然后逐步重构将其提升到生产级别，避免对环境（路径、操作系统等）的任何依赖。
<br/>


❌ **否则：** 你会错过几十年总结下来的人森经验

<br/><br/>


# 第2️⃣章：后端测试

## ⚪ ️2.1 丰富你的测试组合：超脱单元测试和测试金字塔

:white_check_mark: **最佳实践：**[测试金字塔](https://martinfowler.com/bliki/TestPyramid.html)虽然已经有10多年历史了，但仍是一个出色且相关的模型，它提出了3种测试类型，并影响了大多数开发者的测试策略。与此同时，出现了不少亮眼的新测试技术，被隐藏在测试金字塔的阴影下。回顾最近10年来我们看到的所有巨大变化（微服务、云、serverless），有可能会有一个古老的模型适用于*所有*类型的程序吗？测试界不应该考虑欢迎新的测试技术吗？

别误会，在2019年测试金字塔、TDD和单元测试仍然是一项强大的技术，可能是很多程序的最佳方案。和其他模型一样，尽管它们很有用，但有时也是错误的。例如，有一个IOT程序，将许多事件接收到诸如Kafka/RabbitMQ之类的消息总线中，然后流入到某些数据仓库，并最后被一些分析UI所查询。我们是否应该花费50%的测试时间预算，用于编写这种以集成为核心的且几乎没有逻辑的程序的单元测试？随着程序类型的增加（机器人、加密货币、Alexa技能），发现测试金字塔不是最佳方案的机会也越来越大。

是时候丰富你的测试组合并熟悉更多的测试类型了（下一节会有几条建议），还是要注意诸如测试金字塔等模型，但也要让测试类型和你面临的实际问题相匹配（“嘿，我们的API挂了，一起来写一个消费者驱动的合同测试8！”），让你的测试多样化，像一个基于风险分析建立投资组合的投资者一样——评估可能出现的问题并采取一些预防措施来降低这些潜在的风险。

请注意：软件界中TDD理论具有典型的非黑即白现象，一些人鼓吹在所有地方都要用它，但另一些人则觉得它是恶魔。所有持有太绝对观点的人都是不对的: ]。

<br/>


❌ **否则：** 你会错过一些具有惊人ROI（投资回报率）的工具，像Fuzz、lint和mutation等工具可以在10分钟内就提供价值。


<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：Cindy Sridharan在她精彩的文章'Testing Microservices — the sane way'中提出了丰富的测试组合。
![alt text](assets/bp-12-rich-testing.jpeg "Cindy Sridharan suggests a rich testing portfolio in her amazing post ‘Testing Microservices — the sane way’")

<strong class="markup--strong markup--p-strong">☺️示例: </strong><a href="https://www.youtube.com/watch?v=-2zP494wdUY&amp;feature=youtube" data-href="https://www.youtube.com/watch?v=-2zP494wdUY&amp;feature=youtu.be" class="markup--anchor markup--p-anchor" rel="nofollow noopener" target="_blank">[YouTube: “Beyond Unit Tests: 5 Shiny Node.JS Test Types (2018)” (Yoni Goldberg)](https://www.youtube.com/watch?v=-2zP494wdUY&feature=youtu.be)</a>

<br/>

![alt text](assets/bp-12-Yoni-Goldberg-Testing.jpeg "A test name that constitutes 3 parts")


</details>




<br/><br/>

## ⚪ ️2.2 组件测试可能是你的最佳选择

:white_check_mark: **最佳实践：** 每个单元测试只能覆盖应用的一小部分，覆盖全部的代价很高，而端对端测试可以很轻松地覆盖很大部分但既不稳定也慢，所以为什么不采取一种平衡，编写大于单元测试而小于端对端的测试的测试？组件测试是测试界中鲜为人知的一种——它们集中了两边的优点：合理的性能和遵循TDD模式的可能性+现实而广泛的覆盖。

组件测试的重点在微服务的“单元”，它们针对API起作用，不会模拟任何属于微服务本身的东西（比如，真实的数据库，或者至少是该数据库的内存版本），但对所有外部的东西进行stub，比如对其他微服务的调用。这样，我们测试自己部署的内容，从外向内访问程序，并在合理的时间内获得巨大的信心。
<br/>


❌ **否则：** 你可能要花费大量时间编写单元测试并发现只达到了20%的系统覆盖率

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：Supertest允许在进程内访问Express API（快速且覆盖多个层次）

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

![alt text](assets/bp-13-component-test-yoni-goldberg.png " [Supertest](https://www.npmjs.com/package/supertest) allows approaching Express API in-process (fast and cover many layers)")

</details>

<br/><br/>

## ⚪ ️2.3 确保新版本不会破坏现有的API

:white_check_mark: **最佳实践：**  假设你的微服务有多个客户端，并且出于兼容性考虑运行了多个版本的服务（让每个人都满意）。然后你改了什么地方接着“boom！”，有些依赖这些地方的客户端炸了。这是集成世界的Catch-22：服务器端要考虑所有的客户端期望是很难的——另一方面，客户端不可能运行所有的测试因为服务器端把控着发布日期。[消费者主导的合同（测试）以及PACT框架](https://docs.pact.io/)的诞生，就是为了以一种非常有破坏性的方法来让这个过程正式化——不是由服务端来定义自己的测试计划而是由客户端来定义服务端的测试！PACT框架可以记录客户端的期望并放到一个共享的地方：“经纪人（broker）”，所以服务端可以用PACT拉取这些期望并在每次构建时运行，以检查“违约合同”——未满足的客户端期望。如此一来，所有的服务——客户端API错配都会在构建/CI阶段被早早捕获，并可能为你省去很多麻烦。
<br/>


❌ **否则：**累人的人工测试或者部署恐惧

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：

![](https://img.shields.io/badge/🔧%20Example%20using%20PACT-blue.svg
 "Examples with PACT")

![alt text](assets/bp-14-testing-best-practices-contract-flow.png )


</details>



<br/><br/>


## ⚪ ️ 2.4 单独测试你的中间件

:white_check_mark: **最佳实践：**很多人避免进行中间件测试因为它们只占了系统的一小部分并且需要一个Express服务器。这两个理由都是错的——中间件虽然小但会影响所有或者大部分的请求， 并可以作为返回{req, res} JS对象的纯函数被轻松地测试。要测试一个中间件函数，只需要调用它并spy（[例如使用Sinon](https://www.npmjs.com/package/sinon))和{req, res}对象的交互，就可以保证函数功能正常。[node-mock-http](https://www.npmjs.com/package/node-mocks-http)库进一步扩展了Sinon，并把{req, res}对象分解成spy对象。比如，它可以断言res对象上设置的http状态是否符合期望（看下面的例子）
<br/>


❌ **否则：** 一个Express中间件的bug === 一个会发生在所有或大部分请求的bug


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:正确示范：在不发起网络请求和唤醒唤起整个Express服务器的情况下隔离测试中间件

![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with Jest")

```javascript
//the middleware we want to test
const unitUnderTest = require('./middleware')
const httpMocks = require('node-mocks-http');
//Jest syntax, equivelant to describe() & it() in Mocha
test('A request without authentication header, should return http status 403', () => {
  const request = httpMocks.createRequest({
    method: 'GET',
    url: '/user/42',
    headers: {
      authentication: ''
    }
  });
  const response = httpMocks.createResponse();
  unitUnderTest(request, response);
  expect(response.statusCode).toBe(403);
});

```

</details>




<br/><br/>

## ⚪ ️2.5 使用静态分析工具进行测量和重构
:white_check_mark: **最佳实践：**使用静态分析工具可以通过提供客观的方法来提高代码质量并保持代码的可维护性来提供帮助。你可以在CI构建步骤中增加静态分析工具以发现代码有问题时中断。它主要的“卖点”时能在多个文件的上下文中检查代码质量（例如检查重复），执行高阶分析（例如代码复杂性）并跟踪代码issues的历史和进度。其中两种你可以使用的工具是[Sonarqube](https://www.sonarqube.org/) (2,600+ [stars](https://github.com/SonarSource/sonarqube))和[Code Climate](https://codeclimate.com/) (1,500+ [stars](https://github.com/codeclimate/codeclimate))。

来源:: <a href="https://github.com/TheHollidayInn" data-href="https://github.com/TheHollidayInn" class="markup--anchor markup--p-anchor" rel="noopener nofollow" target="_blank">[Keith Holliday](https://github.com/TheHollidayInn)</a>

<br/>


❌ **否则：** 由于糟糕的代码质量，bugs和性能会始终是个没有新库或最新功能可以解决的问题。


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：CodeClimate，一款可以识别复杂方法的收费工具：

![](https://img.shields.io/badge/🔧%20Example%20using%20Code%20Climate-blue.svg
 "Examples with CodeClimate")

![alt text](assets/bp-16-yoni-goldberg-quality.png " CodeClimat, a commercial tool that can identify complex methods:")

</details>




<br/><br/>

## ⚪ ️ 2.6 检查你对Node相关混乱的准备情况
:white_check_mark: **最佳实践：**奇怪的是，大多数软件测试仅涉及逻辑和数据，但有些最糟糕的事情（且真的很难缓解）是基础建设问题。比如，你有测试过当你的进程内存过载、或者服务器/进程挂了，或者你的监控系统发现API慢了50%时，到底发生了什么吗？为了测试和缓解这类糟糕的事情——Netflix提出了 [Chaos engineering](https://principlesofchaos.org/)。它旨在提供意识、框架和工具，来测试我们的程序对混乱问题的适应性。例如，其中一种有名的工具，[chaos monkey](https://github.com/Netflix/chaosmonkey)，会随机地杀掉服务器，以确保我们的服务依然能服务用户，而不依赖于单个服务器（还有个Kubernetes的版本，[kube-monkey](https://github.com/asobti/kube-monkey)，可以杀pods）。所有这些工具都可以在托管/平台级别上工作，但如果你希望测试和生成纯净的的Node混乱，像是检查你的Node进程如何处理未捕获的错误，未处理的promise rejection，在最大允许运存1.7GB下v8内存过载或者当时间循环经常被阻断时你的用户体验是否仍令人满意，这时候怎么办？为了解决上述问题，[node-chaos](https://github.com/i0natan/node-chaos-monkey) (alpha) 提供了Node相关的各种混乱行为。
<br/>


❌ **否则：**  墨菲法则会毫不留情地打击你的生产，无处遁逃


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 最佳实践：Node-chaos可以生成各种Node.js的“恶作剧”，因而你可以测试自己的应用对混乱的复原力
![alt text](assets/bp-17-yoni-goldberg-chaos-monkey-nodejs.png "Node-chaos can generate all sort of Node.js pranks so you can test how resilience is your app to chaos")

</details>

<br/>

## ⚪ ️2.7 避免使用全局的桩数据和数据种子，每个测试单独添加数据

:white_check_mark: **最佳实践：**根据黄金法则（第0章），每个测试应该添加自己的数据库行并只对这些数据进行操作，以防止耦合并便于推断测试流程。实际上，为了提高性能，测试人员经常会在测试运行前种植数据到数据库（也成为“测试桩”），因而违反了这一准则。尽管性能确实是个值得关注的问题——它可以被缓解（详见“组件测试”一章），然而测试的复杂度是个更让人痛苦的问题，应该在大多数情况下比其他因素重要。实际上，应该让每个测试用例显式地添加所需要的数据库记录并只对这些记录进行操作。如果性能成为了关键性的问题——那么一种折中方案就是只给那些不会改变数据的测试套件（例如查询）种植数据。
<br/>


❌ **否则：** 有几个测试失败了，部署被中断，我们的团队将要花费宝贵的时间（去排查），我们是不是遇到bug了？oh no，似乎两个测试在改动同一份种子数据

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例：测试间不是独立的，依赖了一些全局的钩子来提供全局的数据库数据

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Mocha")

```javascript
before(() => {
  //adding sites and admins data to our DB. Where is the data? outside. At some external json or migration framework
  await DB.AddSeedDataFromJson('seed.json');
});
it("When updating site name, get successful confirmation", async () => {
  //I know that site name "portal" exists - I saw it in the seed files
  const siteToUpdate = await SiteService.getSiteByName("Portal");
  const updateNameResult = await SiteService.changeName(siteToUpdate, "newName");
  expect(updateNameResult).to.be(true);
});
it("When querying by site name, get the right site", async () => {
  //I know that site name "portal" exists - I saw it in the seed files
  const siteToCheck = await SiteService.getSiteByName("Portal");
  expect(siteToCheck.name).to.be.equal("Portal"); //Failure! The previous test change the name :[
});

```
<br/>

### :clap: 正确示范：我们可以留在测试范围内，每个测试只操作自己的那份数据

```javascript
it("When updating site name, get successful confirmation", async () => {
  //test is adding a fresh new records and acting on the records only
  const siteUnderTest = await SiteService.addSite({
    name: "siteForUpdateTest"
  });
  const updateNameResult = await SiteService.changeName(siteUnderTest, "newName");
  expect(updateNameResult).to.be(true);
});

```

</details>

<br/><br/>

# 第3️⃣章：前端测试

## ⚪ ️ 3.1. 分离UI和功能

:white_check_mark: **最佳实践：** 当专注于测试组件逻辑时，UI细节是一种需要被排除的噪音，因而你的测试可以专注于纯数据。实际上，应该以一种不太与图形实现相耦合的抽象方式从标记中提取所需的数据，仅对纯数据进行断言（与HTML/CSS的图形细节相比），并禁用会拖慢速度的动画。你可能会想避免只渲染和测试UI的后端部分（例如服务、操作、存储），但这会导致与现实不符的虚构测试，不能揭示正确的数据甚至没有进入UI的情况。


<br/>

❌ **否则：** 你测试中的计算数据可能在10ms内就准备好了，但整个测试由于一些炫酷而不相关的动画要花费500ms（100个测试就要1分钟）

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：分离UI细节

![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React") ![](https://img.shields.io/badge/🔧%20Example%20using%20React%20Testing%20Library-blue.svg
 "Examples with react-testing-library")

```javascript
test('When users-list is flagged to show only VIP, should display only VIP members', () => {
  // Arrange
  const allUsers = [
   { id: 1, name: 'Yoni Goldberg', vip: false }, 
   { id: 2, name: 'John Doe', vip: true }
  ];

  // Act
  const { getAllByTestId } = render(<UsersList users={allUsers} showOnlyVIP={true}/>);

  // Assert - Extract the data from the UI first
  const allRenderedUsers = getAllByTestId('user').map(uiElement => uiElement.textContent);
  const allRealVIPUsers = allUsers.filter((user) => user.vip).map((user) => user.name);
  expect(allRenderedUsers).toEqual(allRealVIPUsers); //compare data with data, no UI here
});

```

<br/>

### :thumbsdown: 反例：断言混合了UI细节和数据
```javascript
test('When flagging to show only VIP, should display only VIP members', () => {
  // Arrange
  const allUsers = [
   {id: 1, name: 'Yoni Goldberg', vip: false }, 
   {id: 2, name: 'John Doe', vip: true }
  ];

  // Act
  const { getAllByTestId } = render(<UsersList users={allUsers} showOnlyVIP={true}/>);

  // Assert - Mix UI & data in assertion
  expect(getAllByTestId('user')).toEqual('[<li data-testid="user">John Doe</li>]');
});

```

</details>




<br/><br/>


## ⚪ ️ 3.2 根据不太可能变动的特性来查询HTML元素

:white_check_mark: **最佳实践：**根据可以在图形变化中保持稳定的特性来查询HTML元素，像是表单标签而非CSS选择器。如果该元素没有这样的特性，那么创建一个专用的测试特性，像是“test-id-submit-button"。遵循这条约定不仅保证了你的功能/逻辑测试不会因为外观和感受变化而失败，而且整个团队都可以清楚地知道这个元素和特性已被测试所使用而不应该被移除。

<br/>

❌ **否则：** 你想要测试一段涵盖了很多组件、逻辑和服务的登录功能，所有东西都布置得很完美——stubs、spies、Ajax调用都是隔离的。一切看上去都是那么的完美。然后测试失败了，因为设计师把div的CSS类从'thick-border'改成了'thin-border'。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：使用专用的测试特性来查询一个元素

![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React")

```html
// the markup code (part of React component)
<h3>
  <Badge pill className="fixed_badge" variant="dark">
    <span data-testid="errorsLabel">{value}</span> <!-- note the attribute data-testid -->
  </Badge>
</h3>
```

```javascript
// this example is using react-testing-library
  test('Whenever no data is passed to metric, show 0 as default', () => {
    // Arrange
    const metricValue = undefined;

    // Act
    const { getByTestId } = render(<dashboardMetric value={undefined}/>);    
    
    expect(getByTestId('errorsLabel')).text()).toBe("0");
  });

```

<br/>

### :thumbsdown: 反例：依赖CSS特性
```html
<!-- the markup code (part of React component) -->
<span id="metric" className="d-flex-column">{value}</span> <!-- what if the designer changes the classs? -->
```

```javascript
// this exammple is using enzyme
test('Whenever no data is passed, error metric shows zero', () => {
    // ...
    
    expect(wrapper.find("[className='d-flex-column']").text()).toBe("0");
  });
```


</details>



<br/>

## ⚪ ️ 3.3 尽可能使用真实且被完全渲染的组件来进行测试

:white_check_mark: **最佳实践：** 只要大小合理，可以像用户一样从外部测试组件，完全渲染UI，对其进行操作并断言渲染的UI如期工作。避免各种模拟、部分和浅渲染——这种方式可能会因为细节不足导致未能捕获一些bugs，并由于会干扰内部而使维护变得困难（见“坚持黑盒测试”一章）。如果其中一个子组件明显拖慢了速度（比如动画）或使得测试布置复杂化——考虑显示地将它替换成伪造的。

话虽如此，但请注意：这种技术适用于子组件被打包成合适大小的小/中型组件。完全渲染一个包含太多子级的组件会导致难以分析测试失败的原因（根本原因的分析），并导致测试变得过慢。在这种情况下，编写少数几个针对大的父组件的测试，以及更多针对子组件的测试。

<br/>

❌ **否则：** 如果通过调用组件的私有方法从而进入组件内部检查它的内部状态——那么当重构组件实现时你将不得不重构所有的测试。你真的有能力进行这种级别的维护吗？

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：使用完全渲染的组件

![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React") ![](https://img.shields.io/badge/🔧%20Example%20using%20Enzyme-blue.svg
 "Examples with Enzyme")

```javascript
class Calendar extends React.Component {
  static defaultProps = {showFilters: false}
  
  render() {
    return (
      <div>
        A filters panel with a button to hide/show filters
        <FiltersPanel showFilter={showFilters} title='Choose Filters'/>
      </div>
    )
  }
}

//Examples use React & Enzyme
test('Realistic approach: When clicked to show filters, filters are displayed', () => {
    // Arrange
    const wrapper = mount(<Calendar showFilters={false} />)

    // Act
    wrapper.find('button').simulate('click');

    // Assert
    expect(wrapper.text().includes('Choose Filter'));
    // This is how the user will approach this element: by text
})


```

### :thumbsdown: 反例：用浅渲染模拟真实环境
```javascript

test('Shallow/mocked approach: When clicked to show filters, filters are displayed', () => {
    // Arrange
    const wrapper = shallow(<Calendar showFilters={false} title='Choose Filter'/>)

    // Act
    wrapper.find('filtersPanel').instance().showFilters();
    // Tap into the internals, bypass the UI and invoke a method. White-box approach

    // Assert
    expect(wrapper.find('Filter').props()).toEqual({title: 'Choose Filter'});
    // what if we change the prop name or don't pass anything relevant?
})

```

</details>

<br/>


## ⚪ ️ 3.4 避免休眠，使用内置支持异步事件的框架。同时尽力加快速度

:white_check_mark: **最佳实践** 在很多情况下，待测单元的完成时间是无法预知的（例如动画会暂停元素的外观）——在这种情况下，避免休眠（例如setTimeOut）并选择大多数平台提供的更具确定性的方法。一些库允许等待操作（例如[Cypress cy.request('url')](https://docs.cypress.io/guides/references/best-practices.html#Unnecessary-Waiting))，另外一些提供用于等待的API，像 [@testing-library/dom method wait(expect(element))](https://testing-library.com/docs/guide-disappearance)。有时，一种更优雅的方法是对慢速资源进行stub，像是API，然后一旦响应时间变得确定组件就可以显式地重新渲染。当依赖于一些休眠的外部组件时，[加快时钟](https://jestjs.io/docs/en/timer-mocks)就变得很有用。休眠时一种应该避免的模式，因为它迫使你的测试变慢或者承受风险（当等待时间太短时）。每当休眠和轮询无法避免并且测试框架没有提供相关支持时，一些npm库比如 [wait-for-expect](https://www.npmjs.com/package/wait-for-expect) 可以提供半确定的解决方案。
<br/>

❌ **否则：** 当长时间休眠时，测试速度将降低一个数量级。当尝试休眠一小段时间时，如果待测单元没有及时响应测试就会失败。因此归根结底这是在脆弱和性能差之间的权衡。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：仅当异步操作完成时才解析的端对端测试API（Cypress）

![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React") ![](https://img.shields.io/badge/🔧%20Example%20using%20React%20Testing%20Library-blue.svg
 "Examples with react-testing-library")

```javascript
// using Cypress
cy.get('#show-products').click()// navigate
cy.wait('@products')// wait for route to appear
// this line will get executed only when the route is ready

```

### :clap: 正确示范：可以等待DOM元素的测试库

```javascript
// @testing-library/dom
test('movie title appears', async () => {
    // element is initially not present...

    // wait for appearance
    await wait(() => {
        expect(getByText('the lion king')).toBeInTheDocument()
    })

    // wait for appearance and return the element
    const movie = await waitForElement(() => getByText('the lion king'))
})

```

### :thumbsdown: 反例：自定义的休眠代码
```javascript

test('movie title appears', async () => {
    // element is initially not present...

    // custom wait logic (caution: simplistic, no timeout)
    const interval = setInterval(() => {
        const found = getByText('the lion king');
        if(found){
            clearInterval(interval);
            expect(getByText('the lion king')).toBeInTheDocument();
        }
        
    }, 100);

    // wait for appearance and return the element
    const movie = await waitForElement(() => getByText('the lion king'))
})

```

</details>


<br/>

## ⚪ ️ 3.5. 监视内容在网络中的传输情况

![](https://img.shields.io/badge/🔧%20Example%20using%20Google%20LightHouse-blue.svg
 "Examples with Lighthouse")

✅ **最佳实践：** 应用一些活动的监视器，保证在真实网络环境下加载的页面得到了优化——包括任何的用户体验问题，像是缓慢的页面加载或者未压缩的打包。有很多检查工具：像[pingdom](https://www.pingdom.com/)、 AWS CloudWatch、 [gcp StackDriver](https://cloud.google.com/monitoring/uptime-checks/)这些基本工具可以被轻松配置，以观察服务器是否处于活动状态并根据合理的SLA做出响应。这些仅能避免浅层的问题，因此最好选择专用于前端的工具（例如 [lighthouse](https://developers.google.com/web/tools/lighthouse/)、[pagespeed](https://developers.google.com/speed/pagespeed/insights/))，并展开更深入的测试。重点应该放在会直接影响用户体验的症状和指标上，比如页面加载时间、[有效渲染（meaninful paint）](https://scotch.io/courses/10-web-performance-audit-tips-for-your-next-billion-users-in-2018/fmp-first-meaningful-paint)、[页面响应时间（TTI）](https://calibreapp.com/blog/time-to-interactive/)。最重要的是，你还要注意一些技术原因，比如确保内容已经压缩、接收第一个字节的时间、优化图片、确保合理的DOM大小、SSL和很多其他东西。建议把这些丰富的监视设置在开发期间、CI的一部分还有最重要的——24x7小时的生产服务器/CDN上。

<br/>

❌ **否则：** 我们会失望地发现，在精心设计UI、100%的功能测试通过和复杂的打包后——用户体验由于CDN配置错误而变得糟糕且缓慢。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

### :clap: 正确示范：Lighthouse页面加载检查报告

![](/assets/lighthouse2.png "Lighthouse page load inspection report")


</details>


<br/>

## ⚪ ️ 3.6 对不稳定和缓慢的资源进行stub，比如后端APIs

:white_check_mark: **最佳实践：**当编写主流测试时（非端对端测试），避免涉及任何超出你负责和控制范围的资源，比如后端API，使用stubs代替（例如测试替身）。实际上，相比于使用真实的网络去调用APIs，应该使用一些测试测试库（如 [Sinon](https://sinonjs.org/)、 [Test doubles](https://www.npmjs.com/package/testdouble)等等）来存根（stubbing）API响应。最主要的好处是防止不稳定性——按定义来测试或staging API不是高度稳定的，会不时让你的测试失败，尽管你的组件表现正常（生产环境并不用于测试，并经常会对请求进行节流）。这么做可以模拟各种API行为，这些行为会驱动你的组件行为，比如没有数据或API抛出错误的情况。最后，网络请求会大大地降低测试速度。

<br/>

❌ **否则：** 平均每个测试的运行时间不会超过几毫秒，但一个典型的API请求时间超过了100毫秒，导致每个测试慢了20倍


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：存根或拦截API调用
![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React") ![](https://img.shields.io/badge/🔧%20Example%20using%20Jest-blue.svg
 "Examples with react-testing-library")

```javascript

// unit under test
export default function ProductsList() { 
    const [products, setProducts] = useState(false)

    const fetchProducts = async() => {
      const products = await axios.get('api/products')
      setProducts(products);
    }

    useEffect(() => {
      fetchProducts();
    }, []);

  return products ? <div>{products}</div> : <div data-testid='no-products-message'>No products</div>
}

// test
test('When no products exist, show the appropriate message', () => {
    // Arrange
    nock("api")
            .get(`/products`)
            .reply(404);

    // Act
    const {getByTestId} = render(<ProductsList/>);

    // Assert
    expect(getByTestId('no-products-message')).toBeTruthy();
});

```

</details>

<br/>

## ⚪ ️ 3.7 只有很少量涵盖整个系统的端对端测试

:white_check_mark: **最佳实践：** 虽然E2E（端对端）测试通常意味着用真实浏览器进行只针对UI的测试（见3.6节），但另一种是对包括后端在内的整个系统进行测试。后一种测试是非常有价值的，因为它们涵盖了由于转换结构的误解（wrong understanding of the exchange schema）而可能发生的前后端集成bugs。它们也是一种发现后端与后端间集成问题的有效方法（例如微服务A发送了一条错误的信息到微服务B），甚至可以检查部署错误——再没有其他像[Cypress](https://www.cypress.io/) 和 [Pupeteer](https://github.com/GoogleChrome/puppeteer)一般友好和成熟的E2E测试框架。这种测试的缺点是配置具有如此多组件的环境的成本很高，而且大多数情况下脆弱性也是如此——给定50个微服务，即使只有一个失败了整个E2E测试也会失败。因此，我们应该谨慎地使用这项技术，可能每个测试1-10个微服务不能再多了。也就是说，津市市少量的E2E测试也可能捕获其要针对的问题——部署或集成错误。建议在类似生产的过渡环境中运行它们。

<br/>

❌ **否则：** 可能会在测试UI的功能上花费大量成本，直到很晚才意识到后端返回的负载（UI使用的数据结构）与预期差距很大

<br/>

## ⚪ ️ 3.8 复用登录凭证加快E2E测试

:white_check_mark: **最佳实践：** 在涉及真实后端并依赖有效的用户令牌进行API调用的E2E测试中，将测试隔离到用户在每次请求中都被创建然后登录的级别是没有必要的。相反，应该在测试开始执行前只登录一次（例如before-all钩子），将令牌保存到一些本地存储中并在请求间复用它。这似乎违反了测试的核心原则之一——避免资源耦合，保持测试自主。尽管这个担忧有道理，但在E2E测试中性能是个关键问题，在开始每个测试前都创建1-3个API请求会导致可拍的执行时间。复用登录凭证并不意味着测试间要操作同一条用户记录——如果依赖用户记录（比如测试用户付款历史），那么确保生成这些记录作为测试的一部分，并避免和其他测试共享它们。同时要记住后端是可以伪造的——如果你的测试是专注于前端，那最好隔离和stub相关后端API（见3.6节）。

<br/>

❌ **否则：** 给定200个测试并假设登录时间100ms，相当于有20秒要用于不断地登录。

<br/>

<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :clap: 正确示范：在before-all中登录而不是before-each

![](https://img.shields.io/badge/🔨%20Example%20using%20Cypress-blue.svg
 "Using Cypress to illustrate the idea")

```javascript
let authenticationToken;

// happens before ALL tests run
before(() => {
  cy.request('POST', 'http://localhost:3000/login', {
    username: Cypress.env('username'),
    password: Cypress.env('password'),
  })
  .its('body')
  .then((responseFromLogin) => {
    authenticationToken = responseFromLogin.token;
  })
})

// happens before EACH test
beforeEach(setUser => () {
  cy.visit('/home', {
    onBeforeLoad (win) {
      win.localStorage.setItem('token', JSON.stringify(authenticationToken))
    },
  })
})

```

</details>




<br/>

## ⚪ ️ 3.9 保留一个专用于跨越站点地图的E2E冒烟测试

:white_check_mark: **最佳实践：** 为了生产环境的监视和开发时的完整性检查，运行一个单独的E2E测试来浏览所有/大部分网页，确保没有一个挂掉。这类测试可以带来巨大的投资回报，因为它们易于编写和维护，但可以检测到任何类型的故障，包括功能、网络和部署问题。其他类型的冒烟和完整性检查都没有这么可靠和详尽——有些团队单纯ping一下主页（生产环境），或有些开发者跑了很多集成测试但没有发现打包和浏览器问题。毋庸置疑，冒烟测试不能代替代替功能测试，只是旨在作为一个快速的烟雾探测器。

<br/>

❌ **否则：** 一切看起来似乎很完美，所有的测试都通过了，生产环境的健康检查也没问题，但付款组件出现了些打包问题，导致/Payment路由没有渲染。


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：跨越所有页面的冒烟测试
![](https://img.shields.io/badge/🔨%20Example%20using%20Cypress-blue.svg
 "Using Cypress to illustrate the idea")
```javascript
it('When doing smoke testing over all page, should load them all successfully', () => {
    // exemplified using Cypress but can be implemented easily
    // using any E2E suite
    cy.visit('https://mysite.com/home');
    cy.contains('Home');
    cy.contains('https://mysite.com/Login');
    cy.contains('Login');
    cy.contains('https://mysite.com/About');
    cy.contains('About');
  })
```

</details>


<br/>

## ⚪ ️ 3.10 将测试作为实时协作文档公开

:white_check_mark: **最佳实践：**除了提高应用的可靠性外，测试还带来了另一个诱人的机会——作为实时应用文档。由于测试本质上是用一种不那么技术性和产品/UX语言来表达的，因此使用恰当的工具可以让它们成为适合所有对象（开发者以及他们的客户）的交流工具。例如，一些框架允许使用人类可读的语言来描述流程和期望（比如测试计划），因此所有的利益相关者，包括产品经理，可以阅读、批准和协作进行测试，成为实时需求文档。这种技术也被称为“验收测试”，因为它允许客户以简明的语言定义自己的验收标准。这是[BDD（行为驱动测试）](https://en.wikipedia.org/wiki/Behavior-driven_development) 的最原始形式。其中一款支持此技术的流行框架是[Cucumber的Javascript版本](https://github.com/cucumber/cucumber-js)，见下面的例子。另一个类似的是[StoryBook](https://storybook.js.org/)，允许将UI组件作为图形目录展示，用户可以浏览每个组件的各种状态（比如，渲染一个不带滤镜的网格，渲染一个带多行或一行都没有的网格，等等），查看它们的外观，以及怎样触发该状态——这也能吸引产品，但大多情况下是作为实时文档服务于使用这些组件的开发人员。

❌ **否则：** 在投入了大量资源进行测试后，不利用好这项便利会是一种遗憾

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：使用cucumber-js来用人类语言描述测试

![](https://img.shields.io/badge/🔨%20Example%20using%20Cocumber-blue.svg  "Examples using Cucumber")
```javascript
// this is how one can describe tests using cucumber: plain language that allows anyone to understand and collaborate

Feature: Twitter new tweet
 
  I want to tweet something in Twitter
  
  @focus
  Scenario: Tweeting from the home page
    Given I open Twitter home
    Given I click on "New tweet" button
    Given I type "Hello followers!" in the textbox 
    Given I click on "Submit" button
    Then I see message "Tweet saved"
    
```

### :clap: 正确示范：使用Storybook来可视化我们的组件、它们的各种状态和输入
![](https://img.shields.io/badge/🔨%20Example%20using%20StoryBook-blue.svg "Using StoryBook")


</details>




## ⚪ ️ 3.11 使用自动化工具检测视觉问题


:white_check_mark: **最佳实践：** 设置自动化工具来在UI发生变化时捕获截图，并检测诸如内容重叠或崩坏等视觉问题。这不仅确保了正确的数据被准备好了，而且用户可以方便地看到它们。这种技术没得到广泛应用，我们的测试思维倾向于功能测试，但这是用户所体验的视觉效果，而且有那么多的设备类型，所以很容易忽略一些令人讨厌的UI bug。一些免费的工具能提供基本功能——生成并保存截图以供人眼检查。尽管这种方法对小型应用来说很有效，但它的缺陷是像其他人工测试一样，当什么时候改动了一些东西时需要人工适配。另一方面，由于缺乏清晰的定义，自动检查UI问题是很有挑战性的——这是“视觉回归”领域的焦点，通过比对旧的UI和最新的改动来检测差异。一些OSS/免费的工具能提供一些此类功能（例如 [wraith](https://github.com/BBC-News/wraith), [PhantomCSS]([https://github.com/HuddleEng/PhantomCSS])），但可能要花费大量的设置时间。收费工具（如 [Applitools](https://applitools.com/), [Percy.io](https://percy.io/))则更进一步，拥有平滑化的安装过程和各种高级功能（如管理UI、警报、通过消除“视觉噪音”（比如广告、动画）来实现智能截图，甚至是对导致问题的DOM/css改动进行根本原因分析）。

<br/>

❌ **否则：** 一张展示了出色内容（通过100%测试）、立即加载完毕但有一半内容被隐藏掉的网页能有多好？


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例：一个典型的视觉回归——正确的内容被糟糕地展示

![alt text](assets/amazon-visual-regression.jpeg "Amazon page breaks")

<br/>


### :clap: 正确示范：配置幽灵来捕获和比较UI快照

![](https://img.shields.io/badge/🔨%20Example%20using%20Wraith-blue.svg
 "Using Cypress to illustrate the idea")

```
# Add as many domains as necessary. Key will act as a label​

domains:
  english: "http://www.mysite.com"​

​# Type screen widths below, here are a couple of examples​

screen_widths:

  - 600​
  - 768​
  - 1024​
  - 1280​


​# Type page URL paths below, here are a couple of examples​
paths:
  about:
    path: /about
    selector: '.about'​
  subscribe:
      selector: '.subscribe'​
    path: /subscribe
```

### :clap: 正确示范：使用Applitools来获得快照比较和其他高级功能

![](https://img.shields.io/badge/🔨%20Example%20using%20AppliTools-blue.svg
 "Using Cypress to illustrate the idea") ![](https://img.shields.io/badge/🔨%20Example%20using%20Cypress-blue.svg
 "Using Cypress to illustrate the idea")

```javascript
import  *  as todoPage from  '../page-objects/todo-page';

describe('visual validation',  ()  =>  {

before(()  =>  todoPage.navigate());

beforeEach(()  =>  cy.eyesOpen({ appName:  'TAU TodoMVC'  }));

afterEach(()  =>  cy.eyesClose());

  

it('should look good',  ()  =>  {

cy.eyesCheckWindow('empty todo list');

  

todoPage.addTodo('Clean room');

  

todoPage.addTodo('Learn javascript');

  

cy.eyesCheckWindow('two todos');

  

todoPage.toggleTodo(0);

  

cy.eyesCheckWindow('mark as completed');

});

});
```




</details>



<br/><br/>


# 第4️⃣章：衡量测试有效性

<br/><br/>

## ⚪ ️ 4.1 获得足以自信的的测试覆盖率，~80%似乎是个幸运数字

:white_check_mark: **最佳实践：** 测试的目的是为了获得足够的信心来快速前行，很想然越多代码经过测试则团队会越有信心。覆盖率是一种衡量有多少行代码（以及分支、语句等）被测试触碰到的指标。所以多少才是足够的？10-30%对理解构建的正确性来说显然太低，另一方面100%的成本又太昂贵，可能会将你的注意力从关键路径转移到代码的奇怪角落。答案是这取决于很多因素，比如应用的类型——如果你在建造下一代的空中客车A380飞机那么必须是100%，但对一个卡通图片网站来说50%可能已经太多了。尽管大部分测试狂热者宣称正确的覆盖率阈值是上下文相关的，但他们大多数也提到了80%这个数字（[Fowler: “in the upper 80s or 90s”](https://martinfowler.com/bliki/TestCoverage.html)）应该能满足大部分的应用。

实现技巧：你可能想给你的持续集成（CI）配置覆盖率阈值([Jest链接](https://jestjs.io/docs/en/configuration.html#collectcoverage-boolean))，并停止不符合此标准的构建（给每个组件配置阈值也是可以的，见下面的例子）。除此之外，还要考虑检测构建覆盖率降低的情况（当一份新提交的代码覆盖率较低时）——这会促使开发者提高或至少保持测试代码的数量。尽管如此，覆盖率只是其中一种衡量维度，一种基于定量的维度，是不足以说明你测试的健壮性的。而且它也可以被伪造，就像下面章节阐述的那样。

<br/>


❌ **否则：**  信心和数字是密不可分的，如果你没测试过系统的大部分——就会有一些恐惧，而恐惧会让你慢下来

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 示例：一份典型的覆盖率报告
![alt text](assets/bp-18-yoni-goldberg-code-coverage.png "A typical coverage report")

<br/>

### :clap: 正确示范：每个组件单独设置覆盖率（使用Jest）

![](https://img.shields.io/badge/🔨%20Example%20using%20Jest-blue.svg
 "Using Cypress to illustrate the idea")

![alt text](assets/bp-18-code-coverage2.jpeg "Setting up coverage per component (using Jest)

</details>



<br/><br/>

## ⚪ ️ 4.2 检查覆盖率报告以发现未测试到的区域和其他异常情况

:white_check_mark: **最佳实践：**有些问题隐藏在雷达之下，使用传统的工具很难发现。这些不是真正的bugs而是出人意料的应用行为，可能会产生严重影响。比如，有些代码区域从来或很少会被调用——你认为'PricingCalculator'类总是设置产品价格，但结果是它从未被调用，尽管我们在数据库中有10000种商品还有很多销售……代码覆盖率报告能帮助你了解应用是否如期运行。除此之外，它还能突出显示哪类代码没被测试——被告知80%的代码被测试过不能说明关键部分是否已经测试过。生成报告很容易——只需要在生产环境运行你的应用，或在测试过程中使用覆盖率追踪工具，然后察看突出显示了每块代码区域被调用频率的彩色报告。如果你花费一些时间看一下这些数据——你可能会发现一些问题。
<br/>


❌ **否则：** 如果你不知道哪些部分的代码没被测试过，你没法知道问题可能出自哪里


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例：这份覆盖率报告的问题在哪里？基于现实生活中的场景，我们在质量检查中跟踪了我们应用的使用情况，并发现了有趣的登录模式（提示：登录失败的数量不成比例，显然在哪里出了问题。最后事实证明，某些前端bug一直攻击后端登录API）

![alt text](assets/bp-19-coverage-yoni-goldberg-nodejs-consultant.png "What’s wrong with this coverage report? based on a real-world scenario where we tracked our application usage in QA and find out interesting login patterns (Hint: the amount of login failures is non-proportional, something is clearly wrong. Finally it turned out that some frontend bug keeps hitting the backend login API)

</details>


<br/><br/>

## ⚪ ️ 4.3 Measure logical coverage using mutation testing

:white_check_mark: **最佳实践：**  传统的覆盖率指标通常是：它可能会向你展示100%的代码覆盖率，但你的函数没有任何一个返回了正确的响应。怎么会这样？它只是衡量测试访问了哪几行代码，但没有检查测试是否测试了所有东西——断言正确的响应。像是一个出差旅行并出示护照印章的人一样——这不能证明他完成了任何工作，只能证明他去过几个机场和酒店。

基于突变的测试（Mutation-based testing）就是通过衡量实际测试的代码量（而不仅是已访问的）来提供帮助。[Stryker](https://stryker-mutator.io/)是一个用于突变测试的JavaScript库，它的实现很简洁：

（1）故意更改代码并“植入bugs”。比如代码newOrder.price===0变成newOrder.price!=0。这些'bugs'被称为突变。

（2）它运行测试，如果所有都成功了那我们就遇到了问题——测试没有达到发现bugs的目的，这种突变就是所谓的存活。如果测试失败了，那么很好，突变被干掉了。

知道所有或大部分突变被干掉比传统的覆盖率有更高的置信度，并且布置（测试）的时间是相近的。

Mutation-based testing is here to help by measuring the amount of code that was actually TESTED not just VISITED. [Stryker](https://stryker-mutator.io/) is a JavaScript library for mutation testing and the implementation is really neat:

(1) it intentionally changes the code and “plants bugs”. For example the code newOrder.price===0 becomes newOrder.price!=0. This “bugs” are called mutations

(2) it runs the tests, if all succeed then we have a problem — the tests didn’t serve their purpose of discovering bugs, the mutations are so-called survived. If the tests failed, then great, the mutations were killed.

<br/>


❌ **否则：** 你会被误导85%的覆盖率意味着你的测试会检查到你85%的代码中的bugs

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例：100%覆盖率, 0%测试

![](https://img.shields.io/badge/🔨%20Example%20using%20Stryker-blue.svg
 "Using Cypress to illustrate the idea")
```javascript
function addNewOrder(newOrder) {
    logger.log(`Adding new order ${newOrder}`);
    DB.save(newOrder);
    Mailer.sendMail(newOrder.assignee, `A new order was places ${newOrder}`);

    return {approved: true};
}

it("Test addNewOrder, don't use such test names", () => {
    addNewOrder({asignee: "John@mailer.com",price: 120});
});//Triggers 100% code coverage, but it doesn't check anything

```
<br/>

### :clap: 正确示范：Stryker报告，一种用于突变测试的工具，检查并计算未经测试的代码量（突变）

![alt text](assets/bp-20-yoni-goldberg-mutation-testing.jpeg "Stryker reports, a tool for mutation testing, detects and counts the amount of code that is not tested (Mutations)")

</details>



<br/><br/>

## ⚪ ️4.4 用测试linters防止测试代码问题

:white_check_mark: **最佳实践：** 有一组专门用于检查测试代码模式并发现问题的ESLint插件。比如，[eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha)会在一个测试被写在全局层级（不是一个describe()语句的子代）或当测试被[跳过](https://mochajs.org/#inclusive-tests)时抛出警告，后者可能导致错误地认为所有测试都通过了。相似的，[eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest)能在一个测试没有一句断言时抛出警告（意味着没有检查任何东西）。

<br/>


❌ **否则：** 看到90%的代码覆盖率和100%的绿色测试会让你脸上露出灿烂的笑容，直到你意识到很多测试没有断言任何东西，且很多测试套件被跳过了。希望你没有基于这些错误的观察部署任何东西。


<br/>
<details><summary>✏ <b>Code Examples</b></summary>
<br/>

### :thumbsdown: 反例：一个充满错误的测试用例，幸运的是所有错误都被Linters捕获了

```javascript
describe("Too short description", () => {
  const userToken = userService.getDefaultToken() // *error:no-setup-in-describe, use hooks (sparingly) instead
  it("Some description", () => {});//* error: valid-test-description. Must include the word "Should" + at least 5 words
});

it.skip("Test name", () => {// *error:no-skipped-tests, error:error:no-global-tests. Put tests only under describe or suite
  expect("somevalue"); // error:no-assert
});

it("Test name", () => {*//error:no-identical-title. Assign unique titles to tests
});
```

</details>

<br/><br/>


# 第5️⃣章: CI和其他质量措施

<br/><br/>

## ⚪ ️ 5.1 丰富你的linters，中断有规则问题的构建

:white_check_mark: **最佳实践：**Linters是免费的午餐，只需5分钟的设置你就能获得免费的自动驾驶仪，保护你的代码并在你键入时捕获重要问题。linting是关于化妆品的日子已经一去不复返了（没有分号！）。现在Linters能捕获严重的问题，比如没有正确抛出的错误以及丢失信息。在基本规则集之上（像 [ESLint standard](https://www.npmjs.com/package/eslint-plugin-standard)或[Airbnb style](https://www.npmjs.com/package/eslint-config-airbnb)），请考虑包括一些特殊的Linters，像能发现测试缺失断言的 [eslint-plugin-chai-expect](https://www.npmjs.com/package/eslint-plugin-chai-expect)，能发现promises没有resolve（你的代码永远不会继续跑）的[eslint-plugin-promise](https://www.npmjs.com/package/eslint-plugin-promise?activeTab=readme)，能发现可能会被利用在DOS攻击的贪婪正则表达的[eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security?activeTab=readme)，以及[eslint-plugin-you-dont-need-lodash-underscore](https://www.npmjs.com/package/eslint-plugin-you-dont-need-lodash-underscore)，能在代码使用了实用方法库中V8核心也自带的方法（如Lodash._map(...)）时发出警报。

  <br/>


❌ **否则：** 一个下雨天，你的产品一直崩掉，但日志不显示错误堆栈的跟踪。发生了啥？你的代码错误地抛出一个非错误对象，且堆栈跟踪丢失了，一个很好的让你撞墙的理由。一个5分钟的linter设置就能检测到这种键入错误并节省你一天时间


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: 反例：错误的Error对象被抛出，这种错误没有堆栈跟踪。幸运的是，ESLint捕获了下一个生产bug
![alt text](assets/bp-21-yoni-goldberg-eslint.jpeg "The wrong Error object is thrown mistakenly, no stack-trace will appear for this error. Luckily, ESLint catches the next production bug")

</details>




<br/><br/>

# ⚪ ️ 5.2 使用本地开发者CI缩短反馈循环

:white_check_mark: **最佳实践：**使用带有质量检测（如测试、linting、漏洞检查等等）的CI？帮助开发者在本地运行此pipeline以征求即时反馈和缩短[反馈循环](https://www.gocd.org/2016/03/15/are-you-ready-for-continuous-delivery-part-2-feedback-loops/)。为什么？一个高效的测试过程包含许多迭代循环：（1）试用->（2）反馈->（3）重构。反馈越迅速，开发者可对每个模块改进的迭代次数就越多，让最终效果更完善。相反，当反馈迟到时，一天内可进行的改进迭代就更少，团队可能已经把注意力放到另一个主题/任务/模块，无法完善该模块。

实际上，一些CI供应商（比如： [CircleCI load CLI](https://circleci.com/docs/2.0/local-cli/)）允许在本地运行pipeline。某些收费工具如[wallaby提供高价值的测试见解](https://wallabyjs.com/)，作为开发者原形（没有从属关系）。此外，你可以单纯在package.json中增加npm脚本运行所有的质量检查命令（如测试、lint、漏洞）——使用像[concurrently](https://www.npmjs.com/package/concurrently)那样的工具进行并行化，如果如果其中一种工具失败则返回非0退出码。现在开发者只需要调用一条命令——如'npm run quality'——就能获得即时反馈。同时考虑使用githook（[husky能帮忙](https://github.com/typicode/husky)）来在质量检查失败时中断提交。
<br/>


❌ **否则：** 如果质量检查结果在代码发布后的第二天才到达，那测试就不是开发中顺其自然的一部分，而是马后炮


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  正确示范：执行代码质量检查的npm脚本在需要时或开发者尝试推送新代码时并行运行
```javascript
"scripts": {
    "inspect:sanity-testing": "mocha **/**--test.js --grep \"sanity\"",
    "inspect:lint": "eslint .",
    "inspect:vulnerabilities": "npm audit",
    "inspect:license": "license-checker --failOn GPLv2",
    "inspect:complexity": "plato .",

    "inspect:all": "concurrently -c \"bgBlue.bold,bgMagenta.bold,yellow\" \"npm:inspect:quick-testing\" \"npm:inspect:lint\" \"npm:inspect:vulnerabilities\" \"npm:inspect:license\""
  },
  "husky": {
    "hooks": {
      "precommit": "npm run inspect:all",
      "prepush": "npm run inspect:all"
    }
}

```

</details>




<br/><br/>

# ⚪ ️5.3 在真实的生产环境镜像上执行e2e测试

:white_check_mark: **最佳实践：**   端对端（e2e）测试时每个CI pipeline上的主要挑战——即时创建具有所有相关云服务的、与生产环境相同的临时镜像可能既繁琐又昂贵。我们要寻找最佳的折中方案：[Docker-compose](https://serverless.com/)允许使用单个文本文件来构建具有相同容器的隔离的dockerized环境，但支持技术（如网络、部署模型）和现实生产环境是不一样的。你可以和[‘AWS Local’](https://github.com/localstack/localstack)结合使用，来使用真实的AWS服务stub。如果你使用[serverless](https://serverless.com/)，那有很多像serverless和[AWS SAM](https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html)等框架允许Faas代码的本地调用。

尽管有很多新工具频繁发布，但庞大的Kubernetes生态还没有正式形成一个标准的本地和CI镜像工具。一种方法是用像[Minikube](https://kubernetes.io/docs/setup/minikube/)和[MicroK8s](https://microk8s.io/)等工具运行一个“小型Kubernetes”，只需要较小的成本就能模拟真实的Kubernetes。另一种方法是在一个远程的“真Kubernetes”上运行测试，一些CI供应商（如[Codefresh](https://codefresh.io/)）已经集成了Kubernetes环境，能轻松地在真正的Kubernetes上运行CI pipeline，其他一些供应商则允许针对远程Kubernetes进行自定义脚本编写。

<br/>


❌ **否则：** 使用不同的技术进行生产和测试需要维护两套部署模型，并使开发者和操作团队分离


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  示例：动态生成Kubernetes集群的CI pipeline<a href="https://container-solutions.com/dynamic-environments-kubernetes/" data-href="https://container-solutions.com/dynamic-environments-kubernetes/" class="markup--anchor markup--p-anchor" rel="noopener nofollow" target="_blank">([来源：Dynamic-environments Kubernetes](https://container-solutions.com/dynamic-environments-kubernetes/))</a>

<pre name="38d9" id="38d9" class="graf graf--pre graf-after--p">deploy:<br>stage: deploy<br>image: registry.gitlab.com/gitlab-examples/kubernetes-deploy<br>script:<br>- ./configureCluster.sh $KUBE_CA_PEM_FILE $KUBE_URL $KUBE_TOKEN<br>- kubectl create ns $NAMESPACE<br>- kubectl create secret -n $NAMESPACE docker-registry gitlab-registry --docker-server="$CI_REGISTRY" --docker-username="$CI_REGISTRY_USER" --docker-password="$CI_REGISTRY_PASSWORD" --docker-email="$GITLAB_USER_EMAIL"<br>- mkdir .generated<br>- echo "$CI_BUILD_REF_NAME-$CI_BUILD_REF"<br>- sed -e "s/TAG/$CI_BUILD_REF_NAME-$CI_BUILD_REF/g" templates/deals.yaml | tee ".generated/deals.yaml"<br>- kubectl apply --namespace $NAMESPACE -f .generated/deals.yaml<br>- kubectl apply --namespace $NAMESPACE -f templates/my-sock-shop.yaml<br>environment:<br>name: test-for-ci</pre>
</details>





<br/><br/>

## ⚪ ️5.4 并行执行测试
:white_check_mark: **最佳实践：** 如果做对了，测试是你的24/7小时朋友，可以提供几乎是立即的反馈。实际上，在单线程上执行500个CPU限制的单元测试会花费很长时间。幸运的是，现代测试运行程序和CI平台（像[Jest](https://github.com/facebook/jest)、[AVA](https://github.com/avajs/ava)和[Mocha扩展](https://github.com/yandex/mocha-parallel-tests)）能把测试并行化为多个进程，从而显著提高反馈时间。一些CI提供商甚至能在多个容器间并行运行测试（！），这进一步缩短了反馈循环。无论是在本地利用多个处理器，还是在云命令行界面上使用多台机器——并行化需要保持测试的自主性，因为每个测试可能在不同的进程上运行。


❌ **否则：** 推送新代码后一个小时才拿到测试结果，此时你已经在写下一个功能了，这是使测试变得不那么相关的一个好办法

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：Mocha parallel和Jest通过测试并行化轻松超越传统的Mocha测试 ([来源：JavaScript Test-Runners Benchmark](https://medium.com/dailyjs/javascript-test-runners-benchmark-3a78d4117b4))
![alt text](assets/bp-24-yonigoldberg-jest-parallel.png "Mocha parallel & Jest easily outrun the traditional Mocha thanks to testing parallelization (Credit: JavaScript Test-Runners Benchmark)")

</details>




<br/><br/>

## ⚪ ️5.5 使用许可证和抄袭检查避免法律问题
:white_check_mark: **最佳实践：** 许可证和抄袭检查可能不是你现在主要关注的问题，但为何不在10分钟内将此复选框也勾上呢？一堆npm包像 [license check](https://www.npmjs.com/package/license-checker)和[plagiarism check](https://www.npmjs.com/package/plagiarism-checker)（收费但有免费计划）可以被轻松地放入CI pipeline中，检查侵权问题，比如包含了有限制性许可证的依赖包，或从Stackoverflow中复制粘贴了代码侵犯了某些版权。

❌ **否则：** 开发者可能会无意中使用了带有不合适许可证的软件包或复制粘贴了商业代码，从而遇到法律问题。


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 正确示范：
```javascript
//install license-checker in your CI environment or also locally
npm install -g license-checker

//ask it to scan all licenses and fail with exit code other than 0 if it found unauthorized license. The CI system should catch this failure and stop the build
license-checker --summary --failOn BSD

```

<br/>

![alt text](assets/bp-25-nodejs-licsense.png)


</details>



<br/><br/>

## ⚪ ️5.6 持续检查脆弱的依赖关系
:white_check_mark:**最佳实践：**即使是最知名的依赖包像Express也会有已知漏洞。使用像[npm audit](https://docs.npmjs.com/getting-started/running-a-security-audit)这种社区工具或[snyk](https://snyk.io/)这种收费工具（也有免费的社区版本）就能轻松解决。两者都能在每次构建时从你的CI调用。 
<br/>


❌ **否则：** 在没有专用工具的情况下保持你的代码无漏洞需要时时关注有关新漏洞的在线刊物。相当乏味


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: 示例：NPM Audit结果
![alt text](assets/bp-26-npm-audit-snyk.png "NPM Audit result")

</details>




<br/><br/>

## ⚪ ️5.7 自动化依赖包升级
:white_check_mark: **最佳实践：** Yarn和npm最新引入的pacakge-lock.json带来了严峻的挑战（通往地狱的路是由善意铺成的）——现在默认情况下，程序包不再得到更新。即使团队用'npm install'和'npm update'运行了很多次重新部署，也不会得到任何更新。这在最好的情况下只导致包版本低于所要求的最低版本，最坏的情况会导致脆弱的代码。团队现在依赖开发者的信誉和记忆来手动更新package.json或手动使用[诸如ncu](https://www.npmjs.com/package/npm-check-updates)等工具。一种更可靠的方法是自动获取最可靠的依赖版本，尽管没有灵丹妙药但还有两条可能的自动化道路：

(1) CI可以让含有过期依赖包的构建失败——使用诸如[‘npm outdated’](https://docs.npmjs.com/cli/outdated)或‘npm-check-updates (ncu)’等工具。这么做可以强迫开发者更新依赖包。

(2) 使用收费工具扫描代码并自动发送已经更新好依赖的pull requests。剩下一个有趣的问题是，依赖更新策略应该是怎样的——在每个补丁版本发布时进行更新开销太大，在主版本更新后立即更新可能会指向不稳定的版本（许多包在发布后的头几天就会被发现漏洞，[见](https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/)eslint-scope事件）。

一个有效的更新策略可能允许一定的“使用期限”——在将本地副本视为过期之前（例如本地版本为1.3.1而仓库版本时1.3.8）让代码落后于@latest一段时间和版本。
<br/>


❌ **否则：** 你的产品会运行着已经被原作者标记为有风险的包


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  示例：可以手动使用[ncu](https://www.npmjs.com/package/npm-check-updates)，或将其内置到CI pipeline中来检查代码落后于最新版本的程度
![alt text](assets/bp-27-yoni-goldberg-npm.png "Nncu can be used manually or within a CI pipeline to detect to which extent the code lag behind the latest versions")


</details>


<br/><br/>

## ⚪ ️ 5.8 其他，与Node无关，CI小技巧
:white_check_mark: **最佳实践：**这篇文章专注于和Node JS相关的测试建议，或者至少能用Node JS举例说明。然而这一节，收集了一些与Node无关但很著名的技巧

 <ol class="postList"><li name="e3e4" id="e3e4" class="graf graf--li graf-after--p">使用声明式语法。这是大多数供应商的唯一选择但老版本的Jenkins允许使用代码或UI</li><li name="1fdc" id="1fdc" class="graf graf--li graf-after--li">选择具有原生Docker支持的供应商</li><li name="edcd" id="edcd" class="graf graf--li graf-after--li">尽早失败，优先运行你最快的测试。在每一步/每个里程碑中创建一个“冒烟测试”，集合多种快速检查（如linting、单元测试），并向代码提交者提供快速的反馈</li><li name="0375" id="0375" class="graf graf--li graf-after--li">使浏览所有的构建工件变得简单，包括测试报告、覆盖率报告、变异报告、日志等等</li><li name="df82" id="df82" class="graf graf--li graf-after--li">为每个事件创建多个pipelines/任务，并复用它们间的步骤。比如，为feature分支的提交配置一个任务，再为master分支的PR配置另一个。让每个复用逻辑使用共享的步骤（大部分供应商提供了一些代码复用的机制）</li><li name="19b0" id="19b0" class="graf graf--li graf-after--li">从不在工作声明中嵌入敏感信息，把它们从一个存储敏感信息的地方或任务配置中拿掉</li><li name="b70d" id="b70d" class="graf graf--li graf-after--li">在发行版本中显式地升级版本，或至少确保开发者这么做了</li><li name="957c" id="957c" class="graf graf--li graf-after--li">仅构建一次，并在单个构建工件上执行所有检查（如Docker镜像）</li><li name="339b" id="339b" class="graf graf--li graf-after--li">在临时环境中进行测试，该环境的状态不会在构建间变化。缓存node_modules可能是唯一的例外</li></ol>

<br/>


❌ **否则：** 你会错过许多人森经验

<br/><br/>

## ⚪ ️ 5.9 构建矩阵：使用多个Node版本运行相同的CI步骤
:white_check_mark: **最佳实践：**质量检查与偶然性相关，你覆盖到的面越多就越容易及早发现问题。当开发可复用的包或运行具有各种配置和Node版本的多客户产品时，CI必须在所有配置组合上运行测试pipeline。例如，假设我们对某些客户使用MySQL而另一些客户使用Postgres——一些CI供应商支持一种叫“矩阵”的功能，允许在所有的MySQL、Postgres和多版本Node（像8、9、10）组合上运行测试套件。只需要配置一下即可，无需任何额外的工作（假设你已经进行测试或任何其他质量检查）。其他一些不支持矩阵的CIs可能会有实现相似功能的扩展或工具箱。
<br/>


❌ **否则：** 在完成了所有那些艰苦的测试编写工作后，我们忍心仅仅因为配置问题而让bugs出现吗？


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:   示例：使用Travis (CI供应商) 的构建定义来在多个Node版本上运行相同的测试
<pre name="f909" id="f909" class="graf graf--pre graf-after--p">language: node_js<br>node_js:<br>  - "7"<br>  - "6"<br>  - "5"<br>  - "4"<br>install:<br>  - npm install<br>script:<br>  - npm run test</pre>
</details>

<br/><br/>

# Team



## Yoni Goldberg

<br/>
<img width="480px" src="assets/yoni-goldberg.jpg"/>
<br/>

**Role:** Writer

**About:** I'm an independent consultant who works with 500 fortune corporates and garage startups on polishing their JS & Node.js applications. More than any other topic I'm fascinated by and aims to master the art of testing. I'm also the author of [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

<br/>

**Workshop:** 👨‍🏫 Want to learn all these practices and techniques at your offices (Europe & USA)? [Register here for my testing workshop](https://testjavascript.com/)
<br/>

**Follow:**

* [🐦 Twitter](https://twitter.com/goldbergyoni/)
* [📞 Contact](https://testjavascript.com/contact-2/)
* [✉️ Newsletter](https://testjavascript.com/newsletter//)

<br/>
<hr/>
<br/>


##  [Bruno Scheufler](https://github.com/BrunoScheufler)

**Role:** Tech reviewer and advisor

Took care to revise, improve, lint and polish all the texts 

**About:** full-stack web engineer, Node.js & GraphQL enthusiast
<hr/>
<br/>

## [Ido Richter](https://github.com/idori)

**Role:** Concept, design and great advice

**About:** A savvy frontend developer, CSS expert and emojis freak
