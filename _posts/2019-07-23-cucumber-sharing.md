---
layout: post
title: Cucumber 分享
---

## 测试与开发 - 狗拿耗子，多管闲事

- 开发需要懂测试吗？
  - 不懂也没关系，懂一点更好。可以帮助自己更好地理解和交付代码。
- 开发写测试代码有什么好处？
  - 自动化测试能带来不少的好处，其中一个是解放人力，让人能够参与到更有价值的工作上面去。具体一点就是，能将人从重复性的劳动（千篇一律的回归测试）中解放出来，参与到更具有创作性的工作（为新功能写新的测试用例）中去。

## Cucumber - 我是一根快乐的小黄瓜

> Cucumber is a tool that supports Behaviour-Driven Development(BDD).  
> —— 引用自 [Cucumber Introduction](https://cucumber.io/docs/guides/overview/)

### BDD

先讲讲 BDD（Behavior-driven development）。

> 行为驱动开发（英语：Behavior-driven development，缩写BDD）是一种敏捷软件开发的技术，它鼓励软件项目中的开发者、QA和非技术人员或商业参与者之间的协作。  
> BDD的重点是通过与利益相关者的讨论取得对预期的软件行为的清醒认识。它通过用自然语言书写非程序员可读的测试用例扩展了测试驱动开发方法。行为驱动开发人员使用混合了领域中统一的语言的母语语言来描述他们的代码的目的。这让开发者得以把精力集中在代码应该怎么写，而不是技术细节上，而且也最大程度的减少了将代码编写者的技术语言与商业客户、用户、利益相关者、项目管理者等的领域语言之间来回翻译的代价。  
> —— 引用自 Wiki: [行为驱动开发](https://zh.wikipedia.org/wiki/%E8%A1%8C%E4%B8%BA%E9%A9%B1%E5%8A%A8%E5%BC%80%E5%8F%91)

### 使用

Cucumber 在他们官网上面有一个[《10 分钟入门教程》](https://cucumber.io/docs/guides/10-minute-tutorial/)。

#### 基本组成

集成 Cucumber 最重要的一个部分就是他的 Feature 文件，以 `.feature` 结尾，如 `hello.feature`。里面详细定义了测试用例。

- Feature，特性。描述需要测试的某一个特性
- Scenario，场景。指定特定的场景
- Step，步骤
  - Given，前提条件
  - When，发起行为
  - Then，获取结果
  - And, But，前一个步骤的延续或者反转
- Backgroup，背景。如果在一个特性里面需要用到同样的前提条件，可以提炼为背景。背景在当前特性的所有场景生效。

#### 步骤定义

Feature 文件其实就是一个测试用例的定义文件，但是没有真正的测试代码，所以需要我们去实现，称之为 `StepDef`。  
在步骤定义里面也基本分为两个部分：

- 代码实现
- 与步骤关联

#### 运行

通过运行 `mvn test` 命令就可以触发运行测试用例

### 使用场景

什么情况下该使用 Cucumber 进行测试呢？

- 总体功能稳定，代码更新较为频繁

---

参考文献：

> [Cucumber (software)](https://en.wikipedia.org/wiki/Cucumber_(software))  
> [Gherkin Reference](https://cucumber.io/docs/gherkin/reference/)  
> [行为驱动测试](https://zh.wikipedia.org/wiki/%E8%A1%8C%E4%B8%BA%E9%A9%B1%E5%8A%A8%E5%BC%80%E5%8F%91)  
> [10 Minute Tutorial](https://cucumber.io/docs/guides/10-minute-tutorial/)  
