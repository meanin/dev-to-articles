---
title: Help me improve my unit tests
published: true
description: Should I use should
tags: #discuss #learning #testing
---

I am .net developer. I have been using TDD on my daily basis for 3 years now. From time to time, I am finding something new. Something what can possibly improve my skills and even give benefits for my employer. I am also a fluent syntax enthusiast, I use [Moq](https://github.com/Moq/moq4/wiki/Quickstart) for creating test doubles and [fluentassertions](http://fluentassertions.com/) for test assertions.

#Test layouts

I like including comments following AAA (Arrange, Act, Assert) in my test methods. It makes me think about what is necessary in arrange section, what in act and in assert one as well. I always try to extract as much as possible from assert and put it into arrange section. In example below, there are input value of tested method and expected output extracted. 
```C#
        [Fact]
        public void GetValue_WhenGivenNumberIs3_ShouldReturnFizz()
        {
            // arrange

            var fizzBuzz = new FizzBuzz.FizzBuzz();
            const int number = 3;
            const string expected = "Fizz";

            // act

            var result = fizzBuzz.GetValue(number);

            // assert

            result.Should().BeEquivalentTo(expected);
        }
```
The benefit of this extraction is a very well-composed assert section. We do not pay for each letter in our version control system, so for me, it looks like a default approach.

Do you also include this kind of comments in your tests?


#Test libraries


Creating test libraries I name unit tests with suffix `.Tests`, and integration tests with suffix `.IntegrationTests`. First part of library name is name of tested library. I usually create test class (file) for single source code class. Rarely, when tested class is big enough, or there are so many case scenarios, I split it into several files, each for one method, or for group of methods which follows similar scenarios. 

What are your feelings about test libraries? Is there something that I missed?


#Naming conventions

Recently, I had a discussion with my coworkers about test method naming convention. We argued about keeping or leaving word `"Should"` in test method name. This conversation left a deep scratch in my mind, I have been thinking about it so many times in the [evenings](https://dev.to/meanin/how-to-deal-with-evenings-bursts-of-creativity-pc) after work. In our company we are following convention from this [article](https://dzone.com/articles/7-popular-unit-test-naming), but personally I like extending it with words `When` and `Should`. Take a closer look on it:

* `{MethodName}_{StateUnderTest}_{ExpectedBehavior}`
```C#
public void GetValue_3_Fizz()
```
* `{MethodName}_When{StateUnderTest}_Should{ExpectedBehavior}`
```C#
public void GetValue_WhenGivenNumberIs3_ShouldReturnFizz()
```

With this approach, name of test method looks like a whole sentence. It makes them self-explanatory even for very young developers. For me, to be honest, test which follows this could be even taken as some kind of a documentation of use cases.

What do you think about it? Should I still use when/should? Would it be good to insist?

And one more question - am I ready to teach younger colleagues in testing approaches?