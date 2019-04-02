---
layout: guide
section: guide
role: page
order: 20
toc: true
title: Nested Suites
label: Nested Suites
description: |
  Nested Suites
---

{:.toc}
## Nested Suites 

Just like your favourite test framework, `Testdeck` supports nested suites.

While in your favourite test framework you might be using nested `describe`, when using `Testdeck`
you should use `suite` instead. And while you can still use `describe`, you should use `suite`, only.
The rationale here being that we may add support for test frameworks that do not support `describe`.

{% highlight TypeScript lineos %}
import { suite, test } from '@testdeck/mocha';

describe('a suite', function() {

  @suite
  class NestedSuite {

    @test
    failingTest() {

      throw new Error('not implemented yet');
    }
  }
});
{% endhighlight %}

Let's replace the `describe` by `suite` and add an extra level of nesting.

{% highlight TypeScript lineos %}
import { suite, test } from '@testdeck/jasmine';

suite('a suite', function() {

  suite('nested suite', function () {

    @suite
    class DeeplyNestedSuite {

      @test
      failingTest() {

        throw new Error('not implemented yet');
      }
    }
  }
});
{% endhighlight %}

Also notice that we changed the target test framework from `Mocha` to `Jasmine`.

{:.toc}
## The Power of Nesting

Nesting gives you the power of dynamically generating suites from, say, test fixtures.

{:.toc}
## Suite Inheritance

And while nesting is a powerful mechanism, we want to point out that there is also inheritance.

With `Testdeck` you can define (abstract) base classes from which your concrete test suites will inherit from, regardless
of the language used for implementing such tests.
And given other mechanisms such as `mixins`, you are free to make your test suites work as you need them to.

See [Suite Inheritance](/pages/guide/inheritance).
