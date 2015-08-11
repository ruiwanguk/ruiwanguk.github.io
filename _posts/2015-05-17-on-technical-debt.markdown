---
layout:     post
title:      On Technical Debt
date:       2015-05-17 20:27:19
summary:    What is technical debt? Why is it important? How to handle technical debts?
categories: ProjectManagement
---

### What is Technical Debt?

Technical debt represents the internal quality of a software project. This include too complex code, code duplication, lack of tests, violations found in static analysis and etc.

To facilitate discussions with people from other domains, it is often introduced as a metaphor referencing the financial domain, where one views internal code quality problems as financial debt, the total amount of debt is the effort it would take to clean up the code base. Just like the normal debt that has interests, the longer you wait, the more effort it takes to solve the issue.

### Why is it important?

From software engineering point of view, we all aim for good architecture and clean maintainable code when working on a project, we believe that by improving the quality of the code base will lead to increase in business values in the long run.

If we don't address the technical debts, they will inevitably surface up and backfire at some point. In the worst cases, they can render the whole team standstill.  

### What are the common misunderstandings of Technical Debt?

Technical debts are often viewed as bad, and we should address these issues as early as possible. But is it really true?

First of all, unlike financial debt, you don't have to pay back the technical debts. If one piece of code or a component will remain unchanged, then it doesn't make sense to re-factor the code, it will bring nothing in return for the business. Like any other long term software project, my team also have legacy systems and legacy tools, they are often not less used and causing us and our users no problems, so these cases, it doesn't make sense to rewrite these code to make them better.

Second, like financial debt, it can be a good thing to have technical debt. In finance, if your return in investment is higher than the amount you borrowed plus the interests, then it makes absolute sense to have the debt. Likewise, if you are building a software product and the time to market is very important, then it makes sense to sacrifice the quality for speed. In fact, a lot of successful companies, such Twitter, Facebook, and Amazon, they are constantly rewriting their system, but their time-to-market has yielded huge financial rewards.

### How should we approach technical debts?

Technical debts are unavoidable! For every project, there will be multiple external impact factors. There may well be time pressure, where developers are forced to make short-term gain by sacrificing the quality. Also, the experience of the team is not equal, there will be some senior developers and some junior developers, they will not produce the same level of quality code. Lastly, the technology is constantly improving, new techniques are created all the time. Even if your code is well designed, overtime, it could be that a new technique comes along and renders the code obsolete.  

Rather than view technical debts as bad, one should view them as investment in the product. Investment in quality will have its benefits, at the same time, it will have associated cost. If the benefits out weight the cost, then the investment in quality should be made. However, if the cost is far higher than the benefits, then one should think again before act.


### Related links

[Technical debt](http://martinfowler.com/bliki/TechnicalDebt.html)

[Managing technical debt](http://www.infoq.com/articles/managing-technical-debt)

[No more technical debt - investment in quality](http://www.infoq.com/articles/no-more-technical-debt)

[Technical debt - Software Engineering Radio](http://www.se-radio.net/2015/04/episode-224-sven-johann-and-eberhard-wolff-on-technical-debt/)
