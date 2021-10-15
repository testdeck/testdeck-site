---
layout: guide
section: guide
role: page
order: 30
toc: true
title: Test Context
label: Test Context
description: |
  Test Context
---

{:.toc}
## Test Context

For functional mocha tests, test and suite contexts are passed through the `this` to the test and suite functions.

In `@testdeck/mocha` these are available on the class and instance objects, assigned on a `context` symbol.

Here is how to access them:

{% highlight TypeScript lineos %}
mport { suite, test, context } from "@testdeck/mocha";

@suite class SuiteContext {
    static async before() {
        this[context].timeout(50);
    }
    @test
    testHasContext(done) {
        this[context].timeout(50);
    }
}
{% endhighlight %}
