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


# Section 2️⃣: Backend Testing

## ⚪ ️2.1 Enrich your testing portfolio: Look beyond unit tests and the pyramid

:white_check_mark: **Do:**  The [testing pyramid](https://martinfowler.com/bliki/TestPyramid.html), though 10> years old, is a great and relevant model that suggests three testing types and influences most developers’ testing strategy. At the same time, more than a handful of shiny new testing techniques emerged and are hiding in the shadows of the testing pyramid. Given all the dramatic changes that we’ve seen in the recent 10 years (Microservices, cloud, serverless), is it even possible that one quite-old model will suit *all* types of applications? shouldn’t the testing world consider welcoming new testing techniques?

Don’t get me wrong, in 2019 the testing pyramid, TDD and unit tests are still a powerful technique and are probably the best match for many applications. Only like any other model, despite its usefulness, [it must be wrong sometimes](https://en.wikipedia.org/wiki/All_models_are_wrong). For example, consider an IOT application that ingests many events into a message-bus like Kafka/RabbitMQ, which then flow into some data-warehouse and are eventually queried by some analytics UI. Should we really spend 50% of our testing budget on writing unit tests for an application that is integration-centric and has almost no logic? As the diversity of application types increase (bots, crypto, Alexa-skills) greater are the chances to find scenarios where the testing pyramid is not the best match.

It’s time to enrich your testing portfolio and become familiar with more testing types (the next bullets suggest few ideas), mind models like the testing pyramid but also match testing types to real-world problems that you’re facing (‘Hey, our API is broken, let’s write consumer-driven contract testing!’), diversify your tests like an investor that build a portfolio based on risk analysis — assess where problems might arise and match some prevention measures to mitigate those potential risks

A word of caution: the TDD argument in the software world takes a typical false-dichotomy face, some preach to use it everywhere, others think it’s the devil. Everyone who speaks in absolutes is wrong :]

<br/>


❌ **Otherwise:** You’re going to miss some tools with amazing ROI, some like Fuzz, lint, and mutation can provide value in 10 minutes


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Cindy Sridharan suggests a rich testing portfolio in her amazing post ‘Testing Microservices — the sane way’
![alt text](assets/bp-12-rich-testing.jpeg "Cindy Sridharan suggests a rich testing portfolio in her amazing post ‘Testing Microservices — the sane way’")

<strong class="markup--strong markup--p-strong">☺️Example: </strong><a href="https://www.youtube.com/watch?v=-2zP494wdUY&amp;feature=youtube" data-href="https://www.youtube.com/watch?v=-2zP494wdUY&amp;feature=youtu.be" class="markup--anchor markup--p-anchor" rel="nofollow noopener" target="_blank">[YouTube: “Beyond Unit Tests: 5 Shiny Node.JS Test Types (2018)” (Yoni Goldberg)](https://www.youtube.com/watch?v=-2zP494wdUY&feature=youtu.be)</a>

<br/>

![alt text](assets/bp-12-Yoni-Goldberg-Testing.jpeg "A test name that constitutes 3 parts")


</details>




<br/><br/>

## ⚪ ️2.2 Component testing might be your best affair

:white_check_mark: **Do:** Each unit test covers a tiny portion of the application and it’s expensive to cover the whole, whereas end-to-end testing easily covers a lot of ground but is flaky and slower, why not apply a balanced approach and write tests that are bigger than unit tests but smaller than end-to-end testing? Component testing is the unsung song of the testing world — they provide the best from both worlds: reasonable performance and a possibility to apply TDD patterns + realistic and great coverage.

Component tests focus on the Microservice ‘unit’, they work against the API, don’t mock anything which belongs to the Microservice itself (e.g. real DB, or at least the in-memory version of that DB) but stub anything that is external like calls to other Microservices. By doing so, we test what we deploy, approach the app from outwards to inwards and gain great confidence in a reasonable amount of time.
<br/>


❌ **Otherwise:** You may spend long days on writing unit tests to find out that you got only 20% system coverage


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Supertest allows approaching Express API in-process (fast and cover many layers)

![](https://img.shields.io/badge/🔧%20Example%20using%20Mocha-blue.svg
 "Examples with Jest")

![alt text](assets/bp-13-component-test-yoni-goldberg.png " [Supertest](https://www.npmjs.com/package/supertest) allows approaching Express API in-process (fast and cover many layers)")

</details>

<br/><br/>

## ⚪ ️2.3 Ensure new releases don’t break the API using

:white_check_mark: **Do:**  So your Microservice has multiple clients, and you run multiple versions of the service for compatibility reasons (keeping everyone happy). Then you change some field and ‘boom!’, some important client who relies on this field is angry. This is the Catch-22 of the integration world: It’s very challenging for the server side to consider all the multiple client expectations — On the other hand, the clients can’t perform any testing because the server controls the release dates. [Consumer-driven contracts and the framework PACT](https://docs.pact.io/) were born to formalize this process with a very disruptive approach — not the server defines the test plan of itself rather the client defines the tests of the… server! PACT can record the client expectation and put in a shared location, “broker”, so the server can pull the expectations and run on every build using PACT library to detect broken contracts — a client expectation that is not met. By doing so, all the server-client API mismatches are caught early during build/CI and might save you a great deal of frustration
<br/>


❌ **Otherwise:** The alternatives are exhausting manual testing or deployment fear


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example:

![](https://img.shields.io/badge/🔧%20Example%20using%20PACT-blue.svg
 "Examples with PACT")

![alt text](assets/bp-14-testing-best-practices-contract-flow.png )


</details>



<br/><br/>


## ⚪ ️ 2.4 Test your middlewares in isolation

:white_check_mark: **Do:** Many avoid Middleware testing because they represent a small portion of the system and require a live Express server. Both reasons are wrong — Middlewares are small but affect all or most of the requests and can be tested easily as pure functions that get {req,res} JS objects. To test a middleware function one should just invoke it and spy ([using Sinon for example](https://www.npmjs.com/package/sinon)) on the interaction with the {req,res} objects to ensure the function performed the right action. The library [node-mock-http](https://www.npmjs.com/package/node-mocks-http) takes it even further and factors the {req,res} objects along with spying on their behavior. For example, it can assert whether the http status that was set on the res object matches the expectation (See example below)
<br/>


❌ **Otherwise:** A bug in Express middleware === a bug in all or most requests


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:Doing It Right Example: Testing middleware in isolation without issuing network calls and waking-up the entire Express machine

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

## ⚪ ️2.5 Measure and refactor using static analysis tools
:white_check_mark: **Do:** Using static analysis tools helps by giving objective ways to improve code quality and keep your code maintainable. You can add static analysis tools to your CI build to abort when it finds code smells. Its main selling points over plain linting are the ability to inspect quality in the context of multiple files (e.g. detect duplications), perform advanced analysis (e.g. code complexity) and follow the history and progress of code issues. Two examples of tools you can use are [Sonarqube](https://www.sonarqube.org/) (2,600+ [stars](https://github.com/SonarSource/sonarqube)) and [Code Climate](https://codeclimate.com/) (1,500+ [stars](https://github.com/codeclimate/codeclimate))

Credit:: <a href="https://github.com/TheHollidayInn" data-href="https://github.com/TheHollidayInn" class="markup--anchor markup--p-anchor" rel="noopener nofollow" target="_blank">[Keith Holliday](https://github.com/TheHollidayInn)</a>

<br/>


❌ **Otherwise:** With poor code quality, bugs and performance will always be an issue that no shiny new library or state of the art features can fix


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example:  CodeClimate, a commercial tool that can identify complex methods:

![](https://img.shields.io/badge/🔧%20Example%20using%20Code%20Climate-blue.svg
 "Examples with CodeClimate")

![alt text](assets/bp-16-yoni-goldberg-quality.png " CodeClimat, a commercial tool that can identify complex methods:")

</details>




<br/><br/>

## ⚪ ️ 2.6 Check your readiness for Node-related chaos
:white_check_mark: **Do:** Weirdly, most software testings are about logic & data only, but some of the worst things that happen (and are really hard to mitigate ) are infrastructural issues. For example, did you ever test what happens when your process memory is overloaded, or when the server/process dies, or does your monitoring system realizes when the API becomes 50% slower?. To test and mitigate these type of bad things — [Chaos engineering](https://principlesofchaos.org/) was born by Netflix. It aims to provide awareness, frameworks and tools for testing our app resiliency for chaotic issues. For example, one of its famous tools, [the chaos monkey](https://github.com/Netflix/chaosmonkey), randomly kills servers to ensure that our service can still serve users and not relying on a single server (there is also a Kubernetes version, [kube-monkey](https://github.com/asobti/kube-monkey), that kills pods). All these tools work on the hosting/platform level, but what if you wish to test and generate pure Node chaos like check how your Node process copes with uncaught errors, unhandled promise rejection, v8 memory overloaded with the max allowed of 1.7GB or whether your UX stays satisfactory when the event loop gets blocked often? to address this I’ve written, [node-chaos](https://github.com/i0natan/node-chaos-monkey) (alpha) which provides all sort of Node-related chaotic acts
<br/>


❌ **Otherwise:**  No escape here, Murphy’s law will hit your production without mercy


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: : Node-chaos can generate all sort of Node.js pranks so you can test how resilience is your app to chaos
![alt text](assets/bp-17-yoni-goldberg-chaos-monkey-nodejs.png "Node-chaos can generate all sort of Node.js pranks so you can test how resilience is your app to chaos")

</details>

<br/>

## ⚪ ️2.7 Avoid global test fixtures and seeds, add data per-test

:white_check_mark: **Do:** Going by the golden rule (bullet 0), each test should add and act on its own set of DB rows to prevent coupling and easily reason about the test flow. In reality, this is often violated by testers who seed the DB with data before running the tests (also known as ‘test fixture’) for the sake of performance improvement. While performance is indeed a valid concern — it can be mitigated (see “Component testing” bullet), however, test complexity is a much painful sorrow that should govern other considerations most of the time. Practically, make each test case explicitly add the DB records it needs and act only on those records. If performance becomes a critical concern — a balanced compromise might come in the form of seeding the only suite of tests that are not mutating data (e.g. queries)
<br/>


❌ **Otherwise:** Few tests fail, a deployment is aborted, our team is going to spend precious time now, do we have a bug? let’s investigate, oh no — it seems that two tests were mutating the same seed data


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti Pattern Example: tests are not independent and rely on some global hook to feed global DB data

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

### :clap: Doing It Right Example: We can stay within the test, each test acts on its own set of data

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

# Section 3️⃣: Frontend Testing

## ⚪ ️ 3.1. Separate UI from functionality

:white_check_mark: **Do:** When focusing on testing component logic, UI details become a noise that should be extracted, so your tests can focus on pure data. Practically, extract the desired data from the markup in an abstract way that is not too coupled to the graphic implementation, assert only on pure data (vs HTML/CSS graphic details) and disable animations that slow down. You might get tempted to avoid rendering and test only the back part of the UI (e.g. services, actions, store) but this will result in fictional tests that don't resemble the reality and won't reveal cases where the right data doesn't even arrive in the UI


<br/>

❌ **Otherwise:** The pure calculated data of your test might be ready in 10ms, but then the whole test will last 500ms (100 tests = 1 min) due to some fancy and irrelevant animation


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Separating out the UI details

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

### :thumbsdown: Anti Pattern Example: Assertion mix UI details and data
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


## ⚪ ️ 3.2 Query HTML elements based on attributes that are unlikely to change

:white_check_mark: **Do:** Query HTML elements based on attributes that are likely to survive graphic changes unlike CSS selectors and like form labels. If the designated element doesn't have such attributes, create a dedicated test attribute like 'test-id-submit-button'. Going this route not only ensures that your functional/logic tests never break because of look & feel changes but also it becomes clear to the entire team that this element and attribute are utilized by tests and shouldn't get removed

<br/>

❌ **Otherwise:** You want to test the login functionality that spans many components, logic and services, everything is set up perfectly - stubs, spies, Ajax calls are isolated. All seems perfect. Then the test fails because the designer changed the div CSS class from 'thick-border' to 'thin-border'

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Querying an element using a dedicated attrbiute for testing

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

### :thumbsdown: Anti-Pattern Example: Relying on CSS attributes
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

## ⚪ ️ 3.3 Whenever possible, test with a realistic and fully rendered component

:white_check_mark: **Do:** Whenever reasonably sized, test your component from outside like your users do, fully render the UI, act on it and assert that the rendered UI behaves as expected. Avoid all sort of mocking, partial and shallow rendering - this approach might result in untrapped bugs due to lack of details and harden the maintenance as the tests mess with the internals (see bullet 'Favour blackbox testing'). If one of the child components is significantly slowing down (e.g. animation) or complicating the setup - consider explicitly replacing it with a fake

With all that said, a word of caution is in order: this technique works for small/medium components that pack a reasonable size of child components. Fully rendering a component with too many children will make it hard to reason about test failures (root cause analysis) and might get too slow. In such cases, write only a few tests against that fat parent component and more tests against its children

<br/>

❌ **Otherwise:** When poking into a component's internal by invoking its private methods, and checking the inner state - you would have to refactor all tests when refactoring the components implementation. Do you really have a capacity for this level of maintenance?


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Working realstically with a fully rendered component

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

### :thumbsdown: Anti-Pattern Example: Mocking the reality with shallow rendering
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


## ⚪ ️ 3.4 Don't sleep, use frameworks built-in support for async events. Also try to speed things up

:white_check_mark: **Do:** In many cases, the unit under test completion time is just unknown (e.g. animation suspends element appearance) - in that case, avoid sleeping (e.g. setTimeOut) and prefer more deterministic methods that most platforms provide. Some libraries allows awaiting on operations (e.g. [Cypress cy.request('url')](https://docs.cypress.io/guides/references/best-practices.html#Unnecessary-Waiting)), other provide API for waiting like [@testing-library/dom method wait(expect(element))](https://testing-library.com/docs/guide-disappearance). Sometimes a more elegant way is to stub the slow resource, like API for example, and then once the response moment becomes deterministic the component can be explicitly re-rendered. When depending upon some external component that sleeps, it might turn useful to [hurry-up the clock](https://jestjs.io/docs/en/timer-mocks). Sleeping is a pattern to avoid because it forces your test to be slow or risky (when waiting for a too short period). Whenever sleeping and polling is inevitable and there's no support from the testing framework, some npm libraries like [wait-for-expect](https://www.npmjs.com/package/wait-for-expect) can help with a semi-deterministic solution 
<br/>

❌ **Otherwise:** When sleeping for a long time, tests will be an order of magnitude slower. When trying to sleep for small numbers, test will fail when the unit under test didn't respond in a timely fashion. So it boils down to a trade-off between flakiness and bad performance


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: E2E API that resolves only when the async operations is done (Cypress)

![](https://img.shields.io/badge/🔧%20Example%20using%20React-blue.svg
 "Examples with React") ![](https://img.shields.io/badge/🔧%20Example%20using%20React%20Testing%20Library-blue.svg
 "Examples with react-testing-library")

```javascript
// using Cypress
cy.get('#show-products').click()// navigate
cy.wait('@products')// wait for route to appear
// this line will get executed only when the route is ready

```

### :clap: Doing It Right Example: Testing library that waits for DOM elements

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

### :thumbsdown: Anti-Pattern Example: custom sleep code
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

## ⚪ ️ 3.5. Watch how the content is served over the network

![](https://img.shields.io/badge/🔧%20Example%20using%20Google%20LightHouse-blue.svg
 "Examples with Lighthouse")

✅ **Do:** Apply some active monitor that ensures the page load under real network is optimized - this includes any UX concern like slow page load or un-minified bundle. The inspection tools market is no short: basic tools like [pingdom](https://www.pingdom.com/), AWS CloudWatch, [gcp StackDriver](https://cloud.google.com/monitoring/uptime-checks/) can be easily configured to watch whether the server is alive and response under a reasonable SLA. This only scratches the surface of what might get wrong, hence it's preferable to opt for tools that specialize in frontend (e.g. [lighthouse](https://developers.google.com/web/tools/lighthouse/), [pagespeed](https://developers.google.com/speed/pagespeed/insights/)) and perform richer analysis. The focus should be on symptoms, metrics that directly affect the UX, like page load time, [meaningful paint](https://scotch.io/courses/10-web-performance-audit-tips-for-your-next-billion-users-in-2018/fmp-first-meaningful-paint), [time until the page gets interactive (TTI)](https://calibreapp.com/blog/time-to-interactive/). On top of that, one may also watch for technical causes like ensuring the content is compressed, time to the first byte, optimize images, ensuring reasonable DOM size, SSL and many others. It's advisable to have these rich monitors both during development, as part of the CI and most important - 24x7 over the production's servers/CDN

<br/>

❌ **Otherwise:** It must be disappointing to realize that after such great care for crafting a UI, 100% functional tests passing and sophisticated bundling - the UX is horrible and slow due to CDN misconfiguration

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

### :clap: Doing It Right Example: Lighthouse page load inspection report

![](/assets/lighthouse2.png "Lighthouse page load inspection report")


</details>


<br/>

## ⚪ ️ 3.6 Stub flakky and slow resources like backend APIs

:white_check_mark: **Do:** When coding your mainstream tests (not E2E tests), avoid involving any resource that is beyond your responsibility and control like backend API and use stubs instead (i.e. test double). Practically, instead of real network calls to APIs, use some test double library (like [Sinon](https://sinonjs.org/), [Test doubles](https://www.npmjs.com/package/testdouble), etc) for stubbing the API response. The main benefit is preventing flakiness - testing or staging APIs by definition are not highly stable and from time to time will fail your tests although YOUR component behaves just fine (production env was not meant for testing and it usually throttles requests). Doing this will allow simulating various API behavior that should drive your component behavior as when no data was found or the case when API throws an error. Last but not least, network calls will greatly slow down the tests

<br/>

❌ **Otherwise:** The average test runs no longer than few ms, a typical API call last 100ms>, this makes each test ~20x slower


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Stubbing or intercepting API calls
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

## ⚪ ️ 3.7 Have very few end-to-end tests that spans the whole system

:white_check_mark: **Do:** Although E2E (end-to-end) usually means UI-only testing with a real browser (See bullet 3.6), for other they mean tests that stretch the entire system including the real backend. The latter type of tests is highly valuable as they cover integration bugs between frontend and backend that might happen due to a wrong understanding of the exchange schema. They are also an efficient method to discover backend-to-backend integration issues (e.g. Microservice A sends the wrong message to Microservice B) and even to detect deployment failures - there are no backend frameworks for E2E testing that are as friendly and mature as UI frameworks like [Cypress](https://www.cypress.io/) and [Pupeteer](https://github.com/GoogleChrome/puppeteer). The downside of such tests is the high cost of configuring an environment with so many components, and mostly their brittleness - given 50 microservices, even if one fails then the entire E2E just failed. For that reason, we should use this technique sparingly and probably have 1-10 of those and no more. That said, even a small number of E2E tests are likely to catch the type of issues they are targeted for - deployment & integration faults. It's advisable to run those over a production-like staging environment

<br/>

❌ **Otherwise:** UI might invest much in testing its functionality only to realizes very late that the backend returned payload (the data schema the UI has to work with) is very differnt than expected

<br/>

## ⚪ ️ 3.8 Speed-up E2E tests by reusing login credentials

:white_check_mark: **Do:** In E2E tests that involve a real backend and rely on a valid user token for API calls, it doesn't payoff to isolate the test to a level where a user is created and logged-in in every request. Instead, login only once before the tests execution start (i.e. before-all hook), save the token in some local storage and reuse it across requests. This seem to violate one of the core testing principle - keep the test autonomous without resources coupling. While this is a valid worry, in E2E tests performance is a key concern and creating 1-3 API requests before starting each individial tests might lead to horrible execution time. Reusing credentials doesn't mean the tests have to act on the same user records - if relying on user records (e.g. test user payments history) than make sure to generate those records as part of the test and avoid sharing their existence with other tests. Also remember that the backend can be faked - if your tests are focused on the frontend it might be better to isolate it and stub the backend API (see bullet 3.6). 

<br/>

❌ **Otherwise:** Given 200 test cases and assuming login=100ms = 20 seconds only for logging-in again and again

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Logging-in before-all and not before-each

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

## ⚪ ️ 3.9 Have one E2E smoke test that just travels across the site map

:white_check_mark: **Do:** For production monitoring and development-time sanity check, run a single E2E test that visits all/most of the site pages and ensures no one breaks. This type of test brings a great return on investment as it's very easy to write and maintain, but it can detect any kind of failure including functional, network and deployment issues. Other styles of smoke and sanity checking are not as reliable and exhaustive - some ops teams just ping the home page (production) or developers who run many integration tests which don't discover packaging and browser issues. Goes without saying that the smoke test doesn't replace functional tests rather just aim to serve as a quick smoke detector

<br/>

❌ **Otherwise:** Everything might seem perfect, all tests pass, production health-check is also positive but the Payment component had some packaging issue and only the /Payment route is not rendering


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Smoke travelling across all pages
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

## ⚪ ️ 3.10 Expose the tests as a live collaborative document

:white_check_mark: **Do:** Besides increasing app reliability, tests bring another attractive opportunity to the table - serve as live app documentation. Since tests inherently speak at a less-technical and product/UX language, using the right tools they can serve as a communication artifact that greatly aligns all the peers - developers and their customers. For example, some frameworks allow expressing the flow and expectations (i.e. tests plan) using a human-readable language so any stakeholder, including product managers, can read, approve and collaborate on the tests which just became the live requirements document. This technique is also being referred to as 'acceptance test' as it allows the customer to define his acceptance criteria in plain language. This is [BDD (behavior-driven testing)](https://en.wikipedia.org/wiki/Behavior-driven_development) at its purest form. One of the popular frameworks that enable this is [Cucumber which has a JavaScript flavor](https://github.com/cucumber/cucumber-js), see example below. Another similar yet different opportunity, [StoryBook](https://storybook.js.org/), allows exposing UI components as a graphic catalog where one can walk through the various states of each component (e.g. render a grid w/o filters, render that grid with multiple rows or with none, etc), see how it looks like, and how to trigger that state - this can appeal also to product folks but mostly serves as live doc for developers who consume those components.

❌ **Otherwise:** After investing top resources on testing, it's just a pity not to leverage this investment and win great value


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Describing tests in human-language using cucumber-js

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

### :clap: Doing It Right Example: Visualizing our components, their various states and inputs using Storybook
![](https://img.shields.io/badge/🔨%20Example%20using%20StoryBook-blue.svg "Using StoryBook")


</details>




## ⚪ ️ 3.11 Detect visual issues with automated tools


:white_check_mark: **Do:** Setup automated tools to capture UI screenshots when changes are presented and detect visual issues like content overlapping or breaking. This ensures that not only the right data is prepared but also the user can conveniently see it. This technique is not widely adopted, our testing mindset leans toward functional tests but it's the visuals what the user experience and with so many device types it's very easy to overlook some nasty UI bug. Some free tools can provide the basics - generate and save screenshots for the inspection of human eyes. While this approach might be sufficient for small apps, it's flawed as any other manual testing that demands human labor anytime something changes. On the other hand, it's quite challenging to detect UI issues automatically due to the lack of clear definition - this is where the field of 'Visual Regression' chime in and solve this puzzle by comparing old UI with the latest changes and detect differences. Some OSS/free tools can provide some of this functionality (e.g. [wraith](https://github.com/BBC-News/wraith), [PhantomCSS]([https://github.com/HuddleEng/PhantomCSS](https://github.com/HuddleEng/PhantomCSS)) but might charge signficant setup time. The commercial line of tools (e.g. [Applitools](https://applitools.com/), [Percy.io](https://percy.io/)) takes is a step further by smoothing the installation and packing advanced features like management UI, alerting, smart capturing by elemeinating  'visual noise' (e.g. ads, animations) and even root cause analysis of the DOM/css changes that led to the issue

<br/>

❌ **Otherwise:** How good is a content page that display great content (100% tests passed), loads instantly but half of the content area is hidden?


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti Pattern Example: A typical visual regression - right content that is served badly

![alt text](assets/amazon-visual-regression.jpeg "Amazon page breaks")

<br/>


### :clap: Doing It Right Example: Configuring wraith to capture and compare UI snapshots

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

### :clap: Doing It Right Example: Using Applitools to get snapshot comaprison and other advanced features

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


# Section 4️⃣: Measuring Test Effectiveness

<br/><br/>

## ⚪ ️ 4.1 Get enough coverage for being confident, ~80% seems to be the lucky number

:white_check_mark: **Do:** The purpose of testing is to get enough confidence for moving fast, obviously the more code is tested the more confident the team can be. Coverage is a measure of how many code lines (and branches, statements, etc) are being reached by the tests. So how much is enough? 10–30% is obviously too low to get any sense about the build correctness, on the other side 100% is very expensive and might shift your focus from the critical paths to the exotic corners of the code. The long answer is that it depends on many factors like the type of application — if you’re building the next generation of Airbus A380 than 100% is a must, for a cartoon pictures website 50% might be too much. Although most of the testing enthusiasts claim that the right coverage threshold is contextual, most of them also mention the number 80% as a thumb of a rule ([Fowler: “in the upper 80s or 90s”](https://martinfowler.com/bliki/TestCoverage.html)) that presumably should satisfy most of the applications.

Implementation tips: You may want to configure your continuous integration (CI) to have a coverage threshold ([Jest link](https://jestjs.io/docs/en/configuration.html#collectcoverage-boolean)) and stop a build that doesn’t stand to this standard (it’s also possible to configure threshold per component, see code example below). On top of this, consider detecting build coverage decrease (when a newly committed code has less coverage) — this will push developers raising or at least preserving the amount of tested code. All that said, coverage is only one measure, a quantitative based one, that is not enough to tell the robustness of your testing. And it can also be fooled as illustrated in the next bullets

<br/>


❌ **Otherwise:**  Confidence and numbers go hand in hand, without really knowing that you tested most of the system — there will also be some fear. and fear will slow you down


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Example: A typical coverage report
![alt text](assets/bp-18-yoni-goldberg-code-coverage.png "A typical coverage report")

<br/>

### :clap: Doing It Right Example: Setting up coverage per component (using Jest)

![](https://img.shields.io/badge/🔨%20Example%20using%20Jest-blue.svg
 "Using Cypress to illustrate the idea")

![alt text](assets/bp-18-code-coverage2.jpeg "Setting up coverage per component (using Jest)

</details>



<br/><br/>

## ⚪ ️ 4.2 Inspect coverage reports to detect untested areas and other oddities

:white_check_mark: **Do:** Some issues sneak just under the radar and are really hard to find using traditional tools. These are not really bugs but more of surprising application behavior that might have a severe impact. For example, often some code areas are never or rarely being invoked — you thought that the ‘PricingCalculator’ class is always setting the product price but it turns out it is actually never invoked although we have 10000 products in DB and many sales… Code coverage reports help you realize whether the application behaves the way you believe it does. Other than that, it can also highlight which types of code is not tested — being informed that 80% of the code is tested doesn’t tell whether the critical parts are covered. Generating reports is easy — just run your app in production or during testing with coverage tracking and then see colorful reports that highlight how frequent each code area is invoked. If you take your time to glimpse into this data — you might find some gotchas
<br/>


❌ **Otherwise:** If you don’t know which parts of your code are left un-tested, you don’t know where the issues might come from


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti-Pattern Example: What’s wrong with this coverage report? based on a real-world scenario where we tracked our application usage in QA and find out interesting login patterns (Hint: the amount of login failures is non-proportional, something is clearly wrong. Finally it turned out that some frontend bug keeps hitting the backend login API)

![alt text](assets/bp-19-coverage-yoni-goldberg-nodejs-consultant.png "What’s wrong with this coverage report? based on a real-world scenario where we tracked our application usage in QA and find out interesting login patterns (Hint: the amount of login failures is non-proportional, something is clearly wrong. Finally it turned out that some frontend bug keeps hitting the backend login API)

</details>


<br/><br/>

## ⚪ ️ 4.3 Measure logical coverage using mutation testing

:white_check_mark: **Do:**  The Traditional Coverage metric often lies: It may show you 100% code coverage, but none of your functions, even not one, return the right response. How come? it simply measures over which lines of code the test visited, but it doesn’t check if the tests actually tested anything — asserted for the right response. Like someone who’s traveling for business and showing his passport stamps — this doesn’t prove any work done, only that he visited few airports and hotels.

Mutation-based testing is here to help by measuring the amount of code that was actually TESTED not just VISITED. [Stryker](https://stryker-mutator.io/) is a JavaScript library for mutation testing and the implementation is really neat:

(1) it intentionally changes the code and “plants bugs”. For example the code newOrder.price===0 becomes newOrder.price!=0. This “bugs” are called mutations

(2) it runs the tests, if all succeed then we have a problem — the tests didn’t serve their purpose of discovering bugs, the mutations are so-called survived. If the tests failed, then great, the mutations were killed.

Knowing that all or most of the mutations were killed gives much higher confidence than traditional coverage and the setup time is similar
<br/>


❌ **Otherwise:** You’ll be fooled to believe that 85% coverage means your test will detect bugs in 85% of your code

<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti Pattern Example: 100% coverage, 0% testing

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

### :clap: Doing It Right Example: Stryker reports, a tool for mutation testing, detects and counts the amount of code that is not tested (Mutations)

![alt text](assets/bp-20-yoni-goldberg-mutation-testing.jpeg "Stryker reports, a tool for mutation testing, detects and counts the amount of code that is not tested (Mutations)")

</details>



<br/><br/>

## ⚪ ️4.4 Preventing test code issues with Test linters

:white_check_mark: **Do:**  A set of ESLint plugins were built specifically for inspecting the tests code patterns and discover issues. For example, [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) will warn when a test is written at the global level (not a son of a describe() statement) or when tests are [skipped](https://mochajs.org/#inclusive-tests) which might lead to a false belief that all tests are passing. Similarly, [eslint-plugin-jest](https://github.com/jest-community/eslint-plugin-jest) can, for example, warn when a test has no assertions at all (not checking anything)

<br/>


❌ **Otherwise:** Seeing 90% code coverage and 100% green tests will make your face wear a big smile only until you realize that many tests aren’t asserting for anything and many test suites were just skipped. Hopefully, you didn’t deploy anything based on this false observation


<br/>
<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti Pattern Example: A test case full of errors, luckily all are caught by Linters

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


# Section 5️⃣: CI and Other Quality Measures

<br/><br/>

## ⚪ ️ 5.1 Enrich your linters and abort builds that have linting issues

:white_check_mark: **Do:**  Linters are a free lunch, with 5 min setup you get for free an auto-pilot guarding your code and catching significant issue as you type. Gone are the days where linting was about cosmetics (no semi-colons!). Nowadays, Linters can catch severe issues like errors that are not thrown correctly and losing information. On top of your basic set of rules (like [ESLint standard](https://www.npmjs.com/package/eslint-plugin-standard) or [Airbnb style](https://www.npmjs.com/package/eslint-config-airbnb)), consider including some specializing Linters like [eslint-plugin-chai-expect](https://www.npmjs.com/package/eslint-plugin-chai-expect) that can discover tests without assertions, [eslint-plugin-promise](https://www.npmjs.com/package/eslint-plugin-promise?activeTab=readme) can discover promises with no resolve (your code will never continue), [eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security?activeTab=readme) which can discover eager regex expressions that might get used for DOS attacks, and [eslint-plugin-you-dont-need-lodash-underscore](https://www.npmjs.com/package/eslint-plugin-you-dont-need-lodash-underscore) is capable of alarming when the code uses utility library methods that are part of the V8 core methods like Lodash._map(…)
<br/>


❌ **Otherwise:** Consider a rainy day where your production keeps crashing but the logs don’t display the error stack trace. What happened? Your code mistakenly threw a non-error object and the stack trace was lost, a good reason for banging your head against a brick wall. A 5min linter setup could detect this TYPO and save your day


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :thumbsdown: Anti Pattern Example: The wrong Error object is thrown mistakenly, no stack-trace will appear for this error. Luckily, ESLint catches the next production bug
![alt text](assets/bp-21-yoni-goldberg-eslint.jpeg "The wrong Error object is thrown mistakenly, no stack-trace will appear for this error. Luckily, ESLint catches the next production bug")

</details>




<br/><br/>

# ⚪ ️ 5.2 Shorten the feedback loop with local developer-CI

:white_check_mark: **Do:**   Using a CI with shiny quality inspections like testing, linting, vulnerabilities check, etc? Help developers run this pipeline also locally to solicit instant feedback and shorten the [feedback loop](https://www.gocd.org/2016/03/15/are-you-ready-for-continuous-delivery-part-2-feedback-loops/). Why? an efficient testing process constitutes many and iterative loops: (1) try-outs -> (2) feedback -> (3) refactor. The faster the feedback is, the more improvement iterations a developer can perform per-module and perfect the results. On the flip, when the feedback is late to come fewer improvement iterations could be packed into a single day, the team might already move forward to another topic/task/module and might not be up for refining that module.

Practically, some CI vendors (Example: [CircleCI load CLI](https://circleci.com/docs/2.0/local-cli/)) allow running the pipeline locally. Some commercial tools like [wallaby provide highly-valuable & testing insights](https://wallabyjs.com/) as a developer prototype (no affiliation). Alternatively, you may just add npm script to package.json that runs all the quality commands (e.g. test, lint, vulnerabilities) — use tools like [concurrently](https://www.npmjs.com/package/concurrently) for parallelization and non-zero exit code if one of the tools failed. Now the developer should just invoke one command — e.g. ‘npm run quality’ — to get instant feedback. Consider also aborting a commit if the quality check failed using a githook ([husky can help](https://github.com/typicode/husky))
<br/>


❌ **Otherwise:** When the quality results arrive the day after the code, testing doesn’t become a fluent part of development rather an after the fact formal artifact


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  Doing It Right Example: npm scripts that perform code quality inspection, all are run in parallel on demand or when a developer is trying to push new code
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

# ⚪ ️5.3 Perform e2e testing over a true production-mirror

:white_check_mark: **Do:**   End to end (e2e) testing are the main challenge of every CI pipeline — creating an identical ephemeral production mirror on the fly with all the related cloud services can be tedious and expensive. Finding the best compromise is your game: [Docker-compose](https://serverless.com/) allows crafting isolated dockerized environment with identical containers using a single plain text file but the backing technology (e.g. networking, deployment model) is different from real-world productions. You may combine it with [‘AWS Local’](https://github.com/localstack/localstack) to work with a stub of the real AWS services. If you went [serverless](https://serverless.com/) multiple frameworks like serverless and [AWS SAM](https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html) allows the local invocation of Faas code.

The huge Kubernetes eco-system is yet to formalize a standard convenient tool for local and CI-mirroring though many new tools are launched frequently. One approach is running a ‘minimized-Kubernetes’ using tools like [Minikube](https://kubernetes.io/docs/setup/minikube/) and [MicroK8s](https://microk8s.io/) which resemble the real thing only come with less overhead. Another approach is testing over a remote ‘real-Kubernetes’, some CI providers (e.g. [Codefresh](https://codefresh.io/)) has native integration with Kubernetes environment and make it easy to run the CI pipeline over the real thing, others allow custom scripting against a remote Kubernetes.
<br/>


❌ **Otherwise:** Using different technologies for production and testing demands maintaining two deployment models and keeps the developers and the ops team separated


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  Example: a CI pipeline that generates Kubernetes cluster on the fly <a href="https://container-solutions.com/dynamic-environments-kubernetes/" data-href="https://container-solutions.com/dynamic-environments-kubernetes/" class="markup--anchor markup--p-anchor" rel="noopener nofollow" target="_blank">([Credit: Dynamic-environments Kubernetes](https://container-solutions.com/dynamic-environments-kubernetes/))</a>

<pre name="38d9" id="38d9" class="graf graf--pre graf-after--p">deploy:<br>stage: deploy<br>image: registry.gitlab.com/gitlab-examples/kubernetes-deploy<br>script:<br>- ./configureCluster.sh $KUBE_CA_PEM_FILE $KUBE_URL $KUBE_TOKEN<br>- kubectl create ns $NAMESPACE<br>- kubectl create secret -n $NAMESPACE docker-registry gitlab-registry --docker-server="$CI_REGISTRY" --docker-username="$CI_REGISTRY_USER" --docker-password="$CI_REGISTRY_PASSWORD" --docker-email="$GITLAB_USER_EMAIL"<br>- mkdir .generated<br>- echo "$CI_BUILD_REF_NAME-$CI_BUILD_REF"<br>- sed -e "s/TAG/$CI_BUILD_REF_NAME-$CI_BUILD_REF/g" templates/deals.yaml | tee ".generated/deals.yaml"<br>- kubectl apply --namespace $NAMESPACE -f .generated/deals.yaml<br>- kubectl apply --namespace $NAMESPACE -f templates/my-sock-shop.yaml<br>environment:<br>name: test-for-ci</pre>
</details>





<br/><br/>

## ⚪ ️5.4 Parallelize test execution
:white_check_mark: **Do:**    When done right, testing is your 24/7 friend providing almost instant feedback. In practice, executing 500 CPU-bounded unit test on a single thread can take too long. Luckily, modern test runners and CI platforms (like [Jest](https://github.com/facebook/jest), [AVA](https://github.com/avajs/ava) and [Mocha extensions](https://github.com/yandex/mocha-parallel-tests)) can parallelize the test into multiple processes and achieve significant improvement in feedback time. Some CI vendors do also parallelize tests across containers (!) which shortens the feedback loop even further. Whether locally over multiple processes, or over some cloud CLI using multiple machines — parallelizing demand keeping the tests autonomous as each might run on different processes


❌ **Otherwise:** Getting test results 1 hour long after pushing new code, as you already code the next features, is a great recipe for making testing less relevant


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example: Mocha parallel & Jest easily outrun the traditional Mocha thanks to testing parallelization ([Credit: JavaScript Test-Runners Benchmark](https://medium.com/dailyjs/javascript-test-runners-benchmark-3a78d4117b4))
![alt text](assets/bp-24-yonigoldberg-jest-parallel.png "Mocha parallel & Jest easily outrun the traditional Mocha thanks to testing parallelization (Credit: JavaScript Test-Runners Benchmark)")

</details>




<br/><br/>

## ⚪ ️5.5 Stay away from legal issues using license and plagiarism check
:white_check_mark: **Do:**    Licensing and plagiarism issues are probably not your main concern right now, but why not tick this box as well in 10 minutes? A bunch of npm packages like [license check](https://www.npmjs.com/package/license-checker) and [plagiarism check](https://www.npmjs.com/package/plagiarism-checker) (commercial with free plan) can be easily baked into your CI pipeline and inspect for sorrows like dependencies with restrictive licenses or code that was copy-pasted from Stackoverflow and apparently violates some copyrights

❌ **Otherwise:** Unintentionally, developers might use packages with inappropriate licenses or copy paste commercial code and run into legal issues


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Doing It Right Example:
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

## ⚪ ️5.6 Constantly inspect for vulnerable dependencies
:white_check_mark: **Do:**    Licensing and plagiarism issues are probably not your main concern right now, but why not tick this box as well in 10 minutes? A bunch of npm packages like license check and plagiarism check (commercial with free plan) can be easily baked into your CI pipeline and inspect for sorrows like dependencies with restrictive licenses or code that was copy-pasted from Stackoverflow and apparently violates some copyrights
<br/>


❌ **Otherwise:** Even the most reputable dependencies such as Express have known vulnerabilities. This can get easily tamed using community tools such as [npm audit](https://docs.npmjs.com/getting-started/running-a-security-audit), or commercial tools like [snyk](https://snyk.io/) (offer also a free community version). Both can be invoked from your CI on every build


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap: Example: NPM Audit result
![alt text](assets/bp-26-npm-audit-snyk.png "NPM Audit result")

</details>




<br/><br/>

## ⚪ ️5.7 Automate dependency updates
:white_check_mark: **Do:**   Yarn and npm latest introduction of package-lock.json introduced a serious challenge (the road to hell is paved with good intentions) — by default now, packages are no longer getting updates. Even a team running many fresh deployments with ‘npm install’ & ‘npm update’ won’t get any new updates. This leads to subpar dependent packages versions at best or to vulnerable code at worst. Teams now rely on developers goodwill and memory to manually update the package.json or use tools [like ncu](https://www.npmjs.com/package/npm-check-updates) manually. A more reliable way could be to automate the process of getting the most reliable dependency versions, though there are no silver bullet solutions yet there are two possible automation roads:

(1) CI can fail builds that have obsolete dependencies — using tools like [‘npm outdated’](https://docs.npmjs.com/cli/outdated) or ‘npm-check-updates (ncu)’ . Doing so will enforce developers to update dependencies.

(2) Use commercial tools that scan the code and automatically send pull requests with updated dependencies. One interesting question remaining is what should be the dependency update policy — updating on every patch generates too many overhead, updating right when a major is released might point to an unstable version (many packages found vulnerable on the very first days after being released, [see the](https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/) eslint-scope incident).

An efficient update policy may allow some ‘vesting period’ — let the code lag behind the @latest for some time and versions before considering the local copy as obsolete (e.g. local version is 1.3.1 and repository version is 1.3.8)
<br/>


❌ **Otherwise:** Your production will run packages that have been explicitly tagged by their author as risky


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:  Example: [ncu](https://www.npmjs.com/package/npm-check-updates) can be used manually or within a CI pipeline to detect to which extent the code lag behind the latest versions
![alt text](assets/bp-27-yoni-goldberg-npm.png "Nncu can be used manually or within a CI pipeline to detect to which extent the code lag behind the latest versions")


</details>


<br/><br/>

## ⚪ ️ 5.8 Other, non-Node related, CI tips
:white_check_mark: **Do:**    This post is focused on testing advice that is related to, or at least can be exemplified with Node JS. This bullet, however, groups few non-Node related tips that are well-known

 <ol class="postList"><li name="e3e4" id="e3e4" class="graf graf--li graf-after--p">Use a declarative syntax. This is the only option for most vendors but older versions of Jenkins allows using code or UI</li><li name="1fdc" id="1fdc" class="graf graf--li graf-after--li">Opt for a vendor that has native Docker support</li><li name="edcd" id="edcd" class="graf graf--li graf-after--li">Fail early, run your fastest tests first. Create a ‘Smoke testing’ step/milestone that groups multiple fast inspections (e.g. linting, unit tests) and provide snappy feedback to the code committer</li><li name="0375" id="0375" class="graf graf--li graf-after--li">Make it easy to skim-through all build artifacts including test reports, coverage reports, mutation reports, logs, etc</li><li name="df82" id="df82" class="graf graf--li graf-after--li">Create multiple pipelines/jobs for each event, reuse steps between them. For example, configure a job for feature branch commits and a different one for master PR. Let each reuse logic using shared steps (most vendors provide some mechanism for code reuse</li><li name="19b0" id="19b0" class="graf graf--li graf-after--li">Never embed secrets in a job declaration, grab them from a secret store or from the job’s configuration</li><li name="b70d" id="b70d" class="graf graf--li graf-after--li">Explicitly bump version in a release build or at least ensure the developer did so</li><li name="957c" id="957c" class="graf graf--li graf-after--li">Build only once and perform all the inspections over the single build artifact (e.g. Docker image)</li><li name="339b" id="339b" class="graf graf--li graf-after--li">Test in an ephemeral environment that doesn’t drift state between builds. Caching node_modules might be the only exception</li></ol>
<br/>


❌ **Otherwise:** You‘ll miss years of wisdom

<br/><br/>

## ⚪ ️ 5.9 Build matrix: Run the same CI steps using multiple Node versions
:white_check_mark: **Do:** Quality checking is about serendipity, the more ground you cover the luckier you get in detecting issues early. When developing reusable packages or running a multi-customer production with various configuration and Node versions, the CI must run the pipeline of tests over all the permutations of configurations. For example, assuming we use MySQL for some customers and Postgres for others — some CI vendors support a feature called ‘Matrix’ which allow running the suit of testing against all permutations of MySQL, Postgres and multiple Node version like 8, 9 and 10. This is done using configuration only without any additional effort (assuming you have testing or any other quality checks). Other CIs who doesn’t support Matrix might have extensions or tweaks to allow that
<br/>


❌ **Otherwise:** So after doing all that hard work of writing testing are we going to let bugs sneak in only because of configuration issues?


<br/>

<details><summary>✏ <b>Code Examples</b></summary>

<br/>

### :clap:   Example: Using Travis (CI vendor) build definition to run the same test over multiple Node versions
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
