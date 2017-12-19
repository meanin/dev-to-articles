---
title: Questions from conducting TDD workshops
published: false
description: TDD workshops
tags: #tdd #teaching #begginers
---

# Introduction

Recently, I have been trying something new in my career. A few weeks ago, I was chosen for a [scrum master role](https://dev.to/meanin/being-a-scrum-master-3p4) for two weeks. It was a nice experience, but now I have next challenge. One of my colleague said, that he appreciated my tips about testing. As a consequence, I was asked to conduct a [TDD workshops](https://github.com/meanin/test-driven-development).

First of all, I introduced several definitions of test-driven development approach and presented people well known diagrams of its influence on long term projects. I said a few words about `red-green-refactor` phases, short development cycles and main test sections. I had prepared project with exercises to consolidate new skills. I presented frameworks for unit testing, FIRST principles, conventions for method names, test doubles and assertions.

# The right part

Let's cut to the chase. 
While teaching guys about testing, I've heard some interesting questions. Here are few of them.

#### 1. How much time does each tdd phase take?

> It depends on how complicated class you are testing. We should aim on making the cycles as short as possible. The shorter cycle we use, the less mistakes we make. It also leads to nice and short tests, which are easy to read.

#### 2. How big arguments range should be tested? In example, should we test the whole integer? From -2,147,483,648 to 2,147,483,647?

> From my experience, the best approach is to test all the edge cases. It is also good to test some dumb values. Nulls are worth to test as well.

#### 3. What is the most reasonable value of the tests coverage?

> Uncle Bob would say - 100%. From my perspective, you can test business logic only, services and stuff like that. If you have a light weight controller, there is not much to test. When you are testing database connection it is in fact an integration test. I would say - 70%. But it depends.

#### 4. Which part of the system should be tested?

> Most of them. The more test you will have, the firmer your system will be. Not every test will be considered as a unit, but in this case, the more the better.

#### 5. What about asynchronous methods, should I test them? How to do that? What about the speed?

> Yes. Most of the testing frameworks contain also asynchronous versions. There is no difficulty to make such a call. When you properly extract abstraction and inject mocked dependencies, there is no worries about the speed.

#### 6. Is there a point to use frameworks like [AutoFixture](https://github.com/AutoFixture/AutoFixture)?

> If you are worried about the obscure test outputs, then you shouldn't use it. As I said earlier, the more test cases you test the more confident you can be about your code. If such a framework would generate unpredictable method input, there is a possibility, to discover the new path in that method. 

#### 7. What is the reason for using the spy test double?

> When you implement a method which runs some dependency several times, the spy will help you. I had a problem with counting how many times a video was played. I extracted abstraction for date and ran several time the same method. Each time my mocked date dependencies had returned different value, so I created a fake process. I simulated the passage of time. With this approach it was really easy to count how many times my video was played.

#### 8. Where is the fine line between the unit and the integration tests?

> When you are injecting any real dependencies instead of mocked one during object creation, then it is an integration test.

#### 9. Are there any alternatives for TDD?

> In .net world, there is a framework called [SpecFlow](http://specflow.org/). It is used to build the application with Behavior-Driven Development (BDD) approach. In this case you are going to create a whole description of the application behavior before starting the real coding. It is an implementation of [cucumber](https://github.com/cucumber/cucumber). Other alternative is to blindly trust your own source code and to wait for Quality Assurance team output. But this is very costly.

#### 10. Does the TDD have any weak point at all?

> Yup, unfortunately. You have to pay high entry cost. Before you start writing your test efficiently, you need to spend many hours writing bad tests, getting familiar with frameworks, etc. Also, a kind of disadvantage is that unit test has to be maintanted.

# Last thoughts

Leading such a training is a nice experience. I am happy, that guys asked me to do it. I faced my fears and I won. I was prepared, topic was well known to me, so basically what to be afraid of? It is easy to be wise after the event, I know. If you ever have such an opportinity, do not hestitate and try yourself.

P.S.

* Are your answers same as mine? 
* Have you heard any other interesting questions about using TDD? 
* What are your experiences with it?