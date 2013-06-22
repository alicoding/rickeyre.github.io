---
layout: post
title: Makefiles... The Horror!... The Power?
tags:
- seneca college
- mozilla
- open-source
status: publish
type: post
published: true
---
In my recent [blog post](/2012/10/04/webvtt-0-1-release-update.html) I mentioned that I had to work with makefiles for the first time in my life. Now, not only am I new to makefiles, but I am new to Linux. I have heard of makefiles before, in my Unix classes at school, but I have not even ran one before much less used them.

This made it hard when I began work on trying to get the makefile in our WebVTT parser to run so that the WebVTT from our .test files would only be ripped if they had changed since the last build time.

Now, because I had never worked with makefiles before I didn't really know how they worked, but I did know what they did. What I knew was that makefiles are a set of instructions for building a project into something, most likely an executable. Even though I didn't know how they worked, I had certain assumptions of how they did. What I assumed was that a makefile was essentially just a bash script, or some kind of programming language, that would somehow build the project. While this assumption was right to an extent i.e. there are some forms of make files that are purely shell script, it was wrong in my case as we were using GNU Make, which does not rely on shell scripts.

All this was very confusing to me because... while because I had no idea what I was doing. I was trying to take cues from the current makefile we were using. I saw that there were parts of it that were shell scripting so for a while I assumed it was all a shell script. After I figured out that most of the lines in the makefile weren't shell scripting I began to look around for stuff that is specific to makefiles that I could use. At this point I did not know there were different versions of make. Therefore, I did not realize that I was trying to use some commands that GNU Make had no idea of. Eventually I clued into this when while talking to my Professor I heard the term GNU Make and he sent me a link to it's documentation. After this I started to put things together and started making progress faster.

I say fast, but lets be realistic, with a thirty year old build system like this a lot of the things that we take for granted in modern day mark up and programming languages such as... oh I don't know, readability, is lacking. To give an example - things like $@, $#, and $(@F) are common to denote 'automatic' variables. These are variables that are built into the make to denote special variables that are relevant to the context of where you put them in the makefile. My Professor, David Humphrey, put it this way - "Every time I have to relearn this it's like drinking rocks". I think that sums it up pretty nicely.

So what is a makefile exactly? A makefile is a file that specifies how to build a target file by denoting its dependencies and the 'recipes' and 'rules' that you need in order build a target from it's dependencies. This gets complicated in the fact that your dependencies will have dependencies and you may have many different rules and recipes to build your many different dependencies. A makefile will also allow you to automate certain things that don't have to do with building a specific target but have to do with managing your build environment. Typical things for managing your build environment that you might see in a makefile are:

* A clean command which usually gets rid of all the 'stuff' that you have created while using the makefile. This allows you to start building your project in a 'clean' environment.
* Commands which allow you to package together thing in your builds such as distributables.
* A check command which allows you to check that all the pre-requisite programs that you need to build your target are available.
* Commands that automate directory building.


A 'rule' and a 'recipe' in a makefile tells the makefile how and when to build a target file. For example:

```
  main.obj : main.c
  bcc –ms –c main.c
```

The rule, first line, tells the makefile that in order to build main.obj it must first have the file main.c. The recipe, second line, tells the makefile that to get from main.c to main.obj it must use the shell command  <em>bcc -ms -c main.c. </em>You can have as many dependencies for one target as you want and your dependencies can have dependencies, so you see how it can get complicated fast.

In the GNU makefile the recipe portion of the make 'command' is basically shell scripting. The rule part of the make command is what is a part of the make syntax and everything below it until the rule of the next command is shell script. It is important to note that you probably want to stay away from any kind of shell script that is specific to certain linux shells so if you're trying to build on another linux shell you won't have any problems.

Another thing that a makefile has are macros. These macros are simliar to the macros in most programming languages in that they let you specify variables ahead of time that can be anything from names of files, to folders, to lists of different things, etc.

Here is an example of a macro from the WebVTT makefile:
```
SRC_DIR = .
TEST_DIR = $(SRC_DIR)/test
```

The final thing that you can use in GNU Make, I don't know about other versions of make, are predefined functions such as the [for each](http://www.gnu.org/software/make/manual/make.html#Foreach-Function) loop that help you transform text. The functions that are provided serve the primary purpose to allow the manipulation of text to format directory or file path names.

One of the things that I heard about makefiles from a friend - the fact that companies hire people just to maintain makefiles, who are called build engineers, makes it sink in even more that makefiles can get extremely complicated.

Even through all my troubles of learning how to use makefiles, I am starting to see how this thirty year old build system, which has it's share of disadvantages, can be an extremely powerful tool in the tool belt of a software engineer.