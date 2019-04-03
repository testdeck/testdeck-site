---
layout: guide
section: guide
role: page 
order: 50
toc: true
title: Tests
label: Tests
description: |
  Tests
---

{:.toc}
## Tests

With `Testdeck` you author tests by decorating methods of your suite with the `@test` decorator.

By using [inheritance](/pages/guide/inheritance#simple-inheritance) you are also able to inherit common tests for a family of SUTs.


{:.toc}
## Synchronous Tests

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import { assert } from 'chai';

@suite
class Suite {

  @test
  test() {

    assert.isOk(false);  
  }
}
{% endhighlight %}

### Run Tests

{% highlight shell %}
npm test

...

  Suite
    1) test

...

  1) Suite
       test:
     AssertionError: expected false to be truthy

...
{% endhighlight %}


{:.toc}
## Asynchronous Tests


{:.toc}
### Callback Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import { assert } from 'chai';

@suite
class Suite {

  @test
  test(done) {

    setTimeout(() => {
    
      try {
      
        assert.isOk(false);
      } catch (err) {
      
        done(err);
      }
    }, 100);  
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  Suite
    1) test

...

  1) Suite
       test:
     AssertionError: expected false to be truthy

...
{% endhighlight %}


{:.toc}
### Promise Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import { assert } from 'chai';

@suite
class Suite {

  @test
  test() {

    return new Promise((resolve, reject) => {

      try {
      
        assert.isOk(false);
        
        resolve(null);
      } catch (err) {
      
        reject(err);
      }
    });  
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  Suite
    1) test

...

  1) Suite
       test:
     AssertionError: expected false to be truthy

...
{% endhighlight %}


{:.toc}
### Async/Await Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import { assert } from 'chai';

@suite
class Suite {

  @test
  async test() {

    return new Promise((resolve, reject) => {

      try {
      
        assert.isOk(false);
        
        resolve(null);
      } catch (err) {
      
        reject(err);
      }
    });  
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  Suite
    1) test

...

  1) Suite
       test:
     AssertionError: expected false to be truthy

...
{% endhighlight %}
