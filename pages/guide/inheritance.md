---
layout: guide
section: guide
role: page
order: 40
toc: true
title: Suite Inheritance
label: Suite Inheritance
description: |
  Suite Inheritance
---

{:.toc}
## Suite Inheritance

In addition to [nesting](/pages/guide/nested), `Testdeck` supports inheritance. And while you may not inherit from
concrete suites, i.e. those that have been decorated with the `@suite` decorator, you can build yourself test base class
hierarchies that provide tests. And, using proper chaining, you can even override existing 
[test lifecycle hooks](/pages/guide/advanced_suite#static-lifecycle-hooks---).


{:.toc}
### Why Limitations On Inheritance?

Deriving from other concrete suites will be detected by `Testdeck` and will be reported as an error.

So don't do this.

{% highlight TypeScript linenos %}
import { suite } from '@testdeck/mocha';

@suite
class SuperSuite {
}

@suite
class ChildSuite extends SuperSuite {
}
{% endhighlight %}

{% highlight shell %}
npm test

...
Error: @suite ChildSuite can not subclass another @suite class, use abstract base class instead.
...

{% endhighlight %}

The rationale behind this is that under the hood, for each `@suite` annotated class, `describe` will be called, which 
then will be passed a callback function that will then do the rest, e.g. register tests, run lifecycle hooks and so on.

Inheriting from another `@suite` annotated class will run this process twice, once for the super class and a second
time for the child class. And this is something that we do not want to happen.


{:.toc}
## Simple Inheritance

TypeScript allows you to define abstract classes that you can use for establishing a common test framework for your
concrete tests to build upon, so we recommend using these as it is more clean approach to OO than inheriting from a 
concrete class. Since the ECMAScript language specification does not support abstract classes yet, you are absolutely
fine subclassing concrete classes, too. For the course of this guide we will stick to abstract classes, though.

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import { expect } from 'chai';

abstract class AbstractTestBase {

  protected abstract createNewInstance();

  @test
  inheritedTest() {
  
    expect(this.createNewInstance()).to.deep.equal({ 'foo': 'bar' });
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  protected createNewInstance() {
  
    return { 'foo': 'bar' };
  }
  
  @test
  specificTest() {
  
    expect(this.createNewInstance().foo).to.contain('bar');
  }  
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
    ✓ specificTest
    ✓ inheritedTest

...
{% endhighlight %}


{:.toc}
## Static Lifecycle Hook Chaining

Static lifecycle hooks are inherited. So if your super class provides a static lifecycle hook, then your child class
will have that hook and you might want to consider overriding it.

However, chaining asynchronous lifecycle hooks is not so straight forward as you might think.


### Callback Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  static before(done) {
  
    console.log('AbstractTestBase.before');  
    return done();
  }
  
  static after(done) {
  
    console.log('AbstractTestBase.after');  
    return done(new Error('AbstractTestBase.after failed'), null);
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  static before(done) {

    // let super before do its thing first   
    super.before((err, res) => {
    
      if (err) {
      
        return done(err);
      }
      
      // do our thing
      console.log('ConcreteTest.before');  

      return done();
    });
  }
  
  static after(done) {

    let err;    
    try {
      
      console.log('ConcreteTest.after');  

      // do our thing

    } catch (ex) {
      
      err = ex;
    } finally {

      return super.after((err2, res) => {
        
        // might want to merge both errors here into one
        return done(err || err2, null);
      });
    }
  }
    
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase.before
ConcreteTest.before
    ✓ test
ConcreteTest.after
AbstractTestBase.after
    1) "after all" hook: after for "test"

...

  1) ConcreteTests
       "after all" hook: after for "test":
     Error: AbstractTestBase.after failed
     
...
{% endhighlight %}


### Promise Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  static before(): Promise<any> {
  
    console.log('AbstractTestBase.before');  
    return Promise.resolve({});
  }
  
  static after(): Promise<any> {

    console.log('AbstractTestBase.after');  
    return Promise.reject(new Error('AbstractTestBase.after failed'));
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  static before(): Promise<any> {

    // let super before do its thing first   
    return super.before()
      .then((res) => {

        return new Promise((resolve, reject) => {
        
          try {
          
            console.log('ConcreteTest.before');  
            // do our thing
        
            resolve(res);
          } catch (err) {
        
            reject(err);  
          }
        });
      }, (err) => {

        return Promise.reject(err);      
      });
  }
  
  static after(): Promise<any> {
    
    try {
      
      console.log('ConcreteTest.after');  

      // do our thing
      
      return super.after();
    } catch (err) {
    
      return Promise.reject(err);
    }
  }
  
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase.before
ConcreteTest.before
    ✓ test
ConcreteTest.after
AbstractTestBase.after
    1) "after all" hook: after for "test"

...

  1) ConcreteTests
       "after all" hook: after for "test":
     Error: AbstractTestBase.after failed
     
...
{% endhighlight %}


### Async/Await Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  static async before(): Promise<any> {
  
    console.log('AbstractTestBase.before');
    return Promise.resolve(null);
  }
  
  static async after(): Promise<any> {
  
    console.log('AbstractTestBase.after');
    return Promise.reject(new Error('AbstractTestBase.after failed'));
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  static async before(): Promise<any> {

    try {
    
      await super.before();

      // do our thing
      console.log('ConcreteTest.before');

      return Promise.resolve(null);      
    } catch (err) {

      return Promise.reject(null);      
    }        
  }

  static async after(): Promise<any> {

    try {
    
      // do our thing
      console.log('ConcreteTest.after');

      await super.after();
      
      return Promise.resolve(null);
    } catch (err) {

      return Promise.reject(err);
    }
  }
  
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase.before
ConcreteTest.before
    ✓ test
ConcreteTest.after
AbstractTestBase.after
    1) "after all" hook: after for "test"

...

  1) ConcreteTests
       "after all" hook: after for "test":
     Error: AbstractTestBase.after failed
     
...
{% endhighlight %}


{:.toc}
## Instance Lifecycle Hook Chaining

Instance lifecycle hooks are inherited. So if your super class provides an instance lifecycle hook, then your child class
will have that hook and you might want to consider overriding it.

The below code is basically the same as shown in [Static Lifecycle Hook Chaining](#static-lifecycle-hook-chaining),
except for the scope of the hook methods.


### Callback Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  before(done) {
  
    console.log('AbstractTestBase#before');  
    return done();
  }
  
  after(done) {
  
    console.log('AbstractTestBase#after');  
    return done(new Error('AbstractTestBase#after failed'), null);
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  before(done) {

    // let super before do its thing first   
    super.before((err, res) => {
    
      if (err) {
      
        return done(err);
      }
      
      // do our thing
      console.log('ConcreteTest#before');  

      return done();
    });
  }
  
  after(done) {

    let err;    
    try {
      
      console.log('ConcreteTest#after');  

      // do our thing

    } catch (ex) {
      
      err = ex;
    } finally {

      return super.after((err2, res) => {
        
        // might want to merge both errors here into one
        return done(err || err2, null);
      });
    }
  }
  
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase#before
ConcreteTest#before
    ✓ test
ConcreteTest#after
AbstractTestBase#after
    1) "after each" hook: after for "test"

...

  1) ConcreteTests
       "after each" hook: after for "test":
     Error: AbstractTestBase#after failed
     
...
{% endhighlight %}


### Promise Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  before(): Promise<any> {
  
    console.log('AbstractTestBase#before');  
    return Promise.resolve({});
  }
  
  after(): Promise<any> {

    console.log('AbstractTestBase#after');  
    return Promise.reject(new Error('AbstractTestBase#after failed'));
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  before(): Promise<any> {

    // let super before do its thing first   
    return super.before()
      .then((res) => {

        return new Promise((resolve, reject) => {
        
          try {
          
            console.log('ConcreteTest#before');  
            // do our thing
        
            resolve(res);
          } catch (err) {
        
            reject(err);  
          }
        });
      }, (err) => {

        return Promise.reject(err);      
      });
  }
  
  after(): Promise<any> {
    
    try {
      
      console.log('ConcreteTest#after');  

      // do our thing
      
      return super.after();
    } catch (err) {
    
      return Promise.reject(err);
    }
  }
  
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase#before
ConcreteTest#before
    ✓ test
ConcreteTest#after
AbstractTestBase#after
    1) "after each" hook: after for "test"

...

  1) ConcreteTests
       "after each" hook: after for "test":
     Error: AbstractTestBase#after failed
     
...
{% endhighlight %}


### Async/Await Style

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

abstract class AbstractTestBase {

  async before(): Promise<any> {
  
    console.log('AbstractTestBase#before');
    return Promise.resolve(null);
  }
  
  async after(): Promise<any> {
  
    console.log('AbstractTestBase#after');
    return Promise.reject(new Error('AbstractTestBase#after failed'));
  }
}

@suite
class ConcreteTests extends AbstractTestBase {

  async before(): Promise<any> {

    try {
    
      await super.before();

      // do our thing
      console.log('ConcreteTest#before');

      return Promise.resolve(null);      
    } catch (err) {

      return Promise.reject(null);      
    }        
  }

  async after(): Promise<any> {

    try {
    
      // do our thing
      console.log('ConcreteTest#after');

      await super.after();
      
      return Promise.resolve(null);
    } catch (err) {

      return Promise.reject(err);
    }
  }
  
  @test
  test() {
  }
}
{% endhighlight %}

#### Run Tests

{% highlight shell %}
npm test

...

  ConcreteTests
AbstractTestBase#before
ConcreteTest#before
    ✓ test
ConcreteTest#after
AbstractTestBase#after
    1) "after each" hook: after for "test"

...

  1) ConcreteTests
       "after each" hook: after for "test":
     Error: AbstractTestBase#after failed
     
...
{% endhighlight %}
