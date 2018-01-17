---
title: Online Judger Design Note 1
date: 2017-03-04 21:17:02
tags:
  - Full-stack
  - Angular2
  - System Design
categories:
  - Web
---

Recently, I am working on a code online judger application as a practice of using full-stack technologies I currently know. This post is development note of this app, including system design, sketch, code implementation, issue/challenges and my solutions.
<!---more--->

## System Design

### Scenario:
What I want to build is a collabrative code judger, which can executed the code pieces that users submit, and show the results of comparison.

### Necessary
I can estimate the data in a very very positive way:<br>

#### Lookup:
Daily active users: 100000<br>
Active users per second: 100000*3(function frequency)/(3600*24) = 3.5
Peak user per second: 3.5*10 = 35

#### Editing
Average concurrent users: 100000*60*60/(3600*24) = 416
QPS: 400*50%(function usage)*10(function frequency) = 2000
Peak Per second: 2000*10 = 20000

#### Submit code
Per second: 400*1%(function usage)*1(function frequency) = 4
Peak per second = 40

## Architecture
This is the draft of overview architecture:

![architecture](https://bittigerimages.s3.amazonaws.com/gitbookImages/CS503/%E8%AF%BE%E7%A8%8B%E5%9B%BE.png)

## First version of front end
So I spent a weekend to learn how to use Angular 2 to design the front end part in a draft review. This first version is a just clone the style of Leetcode.

![page1](http://res.cloudinary.com/dxdsd8err/image/upload/c_scale,h_227,q_70/v1488432522/Screen_Shot_2017-03-01_at_11.18.46_PM_hhukxp.bmp)

![page2](http://res.cloudinary.com/dxdsd8err/image/upload/c_scale,h_266,q_66,w_629/v1488432522/Screen_Shot_2017-02-27_at_1.01.08_AM_ji8mua.bmp)

* commit id：805e50198b0796b9cc7036077a743d9674979ee6

The skills mainly used here are Angular 2, TypeScript, and Bootstrap. I used React.js since 6 months ago, and I feel Angular 2 has more steep learning curve. Many concepts are hard to accept at the first glance.

However, the data binding part makes me confortable because it supports 2-way data binding. Rather than one-way data flow in React, angular 2 provides more options to do bings. Glad to know that.

![page2](http://res.cloudinary.com/dxdsd8err/image/upload/v1488432577/Screen_Shot_2017-02-26_at_1.07.53_PM_bapnvx.bmp)
