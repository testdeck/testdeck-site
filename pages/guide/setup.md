---
layout: guide
section: guide
role: page 
order: 1
toc: true
title: Setting things up
label: Setup
description: |
  Setting things up
---

## Setup 

First, you need to choose which test framework you are going to use.

Testdeck supports Mocha, Jasmine and Jest.

With some limitations, you can always change the test framework later on. (limitations: assertion/expectation framework.)

### Mocha

{% highlight shell %}
npm install @testdeck/mocha mocha @types/mocha
{% endhighlight %}

Alternatively, you can clone the existing <a href="https://github.com/testdeck/testdeck-mocha-seed">mocha-seed</a> and start from there.

### Jasmine

{% highlight bash %}
npm install @testdeck/jasmine jasmine @types/jasmine
{% endhighlight %}

Alternatively, you can clone the existing <a href="https://github.com/testdeck/testdeck-jasmine-seed">jasmine-seed</a> and start from there.

### Jest

{% highlight bash %}
npm install @testdeck/jest jest @types/jest
{% endhighlight %}

Alternatively, you can clone the existing <a href="https://github.com/testdeck/testdeck-jest-seed">jest-seed</a> and start from there.

