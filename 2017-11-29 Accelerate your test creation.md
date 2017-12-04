---
title: Accelerate your test creation
published: true
description: Scafolding for XUnit tests
tags: #tdd #scaffolding #efficiency #visualstudio
---

I want to code efficiently. I am always trying to use TDD approach. For some time I have been wondering, what takes up most of my time during development process. I want to improve my daily routine, to care more about coding itself and less about configuring solutions, projects, classes etc.

#Introduction

I am aware of power of scaffolding, but really, how often do you launch brand new solution? When creating, lets say  web solution, you have to decide if it should be only an API or/and a front-end solution, right? 
![Creating new MVC project](https://thepracticaldev.s3.amazonaws.com/i/mwk7e205ym41ho8h3oie.jpg)
After some time, there is a nice setup solution. With created HomeController, related Views etc. You have something to work with instead of writing everything from a scratch. 
![Created solution](https://thepracticaldev.s3.amazonaws.com/i/t1hv55w15gdlodfke94f.jpg)

But it happens once for a solution lifetime. From the other hand, how often does it occur that you need to write new lib? If you are practicing TDD, there is also an associated test project.

So I was wondering, is there any way to take this functionality and apply it on creation of test project? Because each time I create a class library, I also create a test project. I am using XUnit on my daily basis, so I focused on using it in my little invention.

#First attempt

I did some research to know if there is any option. Scaffolding is a well known functionality, introduced in Visual Studio 2013. It is not so difficult to create custom scaffolded items. There is a way to extract it from existing projects, files, code. Following this way, I created my first templates for Visual Studio. I took vs15 and vs17. You can find my work [here](https://github.com/meanin/xunit-scaffolding). It is nothing advanced, but it fulfills my basic requirements. Trying to be professional, I have made an icon and an installer for this. It creates a project with XUnit nuget preinstalled or a new class with imposed test method layout.
![Scaffolding for XUnit](https://thepracticaldev.s3.amazonaws.com/i/9dsq6urq6lll0dudu0ty.jpg)

It results in a test layout like this below:

```c#
using Xunit;

namespace Xunit_Test_Project1
{
    public class Tests
    {
        [Fact]
        public void Method_When_Should()
        {
            // arrange

            // act

            // assert

        }
    }
}
```
I also created a code snippet for creating test methods - works same as above after typing `testmethod`. There is also a possibility to do that with R# mechanisms.

#Feedback

Next morning, full of the joys of spring, I went to the office and wanted to share my little side project with co-workers. First person I had shared the news with, brought me down to earth. There is already created solution, which provides everything what I need. 

My colleague showed me [NUnit.TestGenerator](https://marketplace.visualstudio.com/items?itemName=NUnitDevelopers.TestGeneratorNUnitextension). Did you know that? He was working with in in his previous company. After playing with it for some time, I agreed that it was very useful and this was what I really needed. My nature didn't give up and was trying to pick holes in this Nunit.TestGenerator. Why? Because it is using NUnit instead of my favorite XUnit. So I started to dig for any equivalent using Xunit.

![Test generator for xunit](https://thepracticaldev.s3.amazonaws.com/i/hlev5zoiamht0h3t40lf.jpg)

Unfortunately, none of these is available on `Extension and Updates` context menu.

#Second attempt

Bearing this in mind for a few days I have been searching for similar solution. I found [XUnit.TestGenerator](https://marketplace.visualstudio.com/items?itemName=YowkoTsai.xUnitnetTestGenerator) online, author - YowkoTsai. It is not available through VS, but you can download an installer.

It works like this - after right-click a method name, shows up a menu, where is a section for test purpose. Without this add-on, there are only `Run tests` and `Debug tests` options. When you install it, you can also find `Create unit tests` options.
![Create tests](https://thepracticaldev.s3.amazonaws.com/i/v1jj151lo5gf03qmsxrc.jpg)
It opens a new dialog window, where you can configure your whole new project and/or classes by clicking on it.
![Create test context](https://thepracticaldev.s3.amazonaws.com/i/lmewc3lw0ekq4t1fd51q.jpg)

Do you think that the journey already ends? Noooooo.

I found it also on github. Forked it. I didn't touch it during first week, but after that, I sat down and tried to enforce a few of my rules for testing.

* Test project naming convention: `.Test` and `.IntegrationTests`
* Test method naming convention: `Method_When_Should`
* Test method layout (as above)
* XUnit of course!

I started with the easiest one, test layout. This was a simple change on one field. Build, install and it works. At least [something](https://github.com/meanin/xUnit.net.TestGenerator) I was able to customize to. I realized that other points were not possible, because of the lack of the documentation, because only a few repository exists on github, etc. Even if I wanted to do that in a proper way, it would take so much time with testing, building and so on. Now I am wondering if Microsoft will eventually give us an opportunity to fully customize tools like this. After all I end up with something like this (slow loading):
![gif](https://thepracticaldev.s3.amazonaws.com/i/381pbt525eiavg7an8k1.gif)

#Summary

I will use my custom test generator. Also, I want to try to convince my co-workers to use it as well. I like to use shared standards in daily work. Is it useful? Will I benefit from it?

I hope that time will show.

---

What are your feelings about this scaffolding in general? Do you have favorite tools for creating tests? Can you help me to push further this customization?

Please, share with me your experience.