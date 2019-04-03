---
layout: guide
section: guide
role: page
order: 30
toc: true
title: Advanced Suites
label: Advanced Suites
description: |
  Advanced Suites
---

## Advanced Suites

{:.toc}
## Suite Execution Options

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite(slow(4000))
@timeout(4000)
class Suite {

  @test
  slowButNotTimedOut(done) {
  
    setTimeout(done, 3500);
  }
  
  @test
  timedOut(done) {
  
    setTimeout(done, 4500);
  }
}
{% endhighlight %}


{:.toc}
### slow {% include support-mocha.html %}

`slow` can be used as either a decorator or passed as a parameter to the `@suite` decorator.

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite(slow(2000))
@slow(2000)
class Suite {

  // ...
}
{% endhighlight %}


{:.toc}
### timeout {% include support-all.html %}

`timeout` can be used as either a decorator or passed as a parameter to the `@suite` decorator.

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite(timeout(2000))
@timeout(2000)
class Suite {

  // ...
}
{% endhighlight %}


{:.toc}
## Suite Naming {% include support-all.html %}

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite("a custom name for a suite")
class Suite {

  @test
  test() {
  }
}
{% endhighlight %}


{:.toc}
## Pending Suites {% include support-all.html %}

{% highlight TypeScript linenos %}
import { suite } from '@testdeck/mocha';

@suite.pending("a pending suite")
class Suite {
}
{% endhighlight %}


{:.toc}
## Skipping Suites {% include support-all.html %}

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite.skip("a skipped suite")
class Suite {

  @test
  test() {
  }
}
{% endhighlight %}


{:.toc}
## Focused Suites {% include support-all.html %}

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

@suite.only("a focused suite")
class Suite {

  @test
  test() {
  }
}
{% endhighlight %}


{:.toc}
## Static Lifecycle Hooks {% include support-all.html %}

`Testdeck` supports static test lifecycle hooks, namely `before` and `after`, which are equivalent to the `beforeAll` 
and `afterAll` test lifecycle hooks found in either of the supported test frameworks. 

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import * as k8s from '@kubernetes/client-node';

@suite
class Suite {

  private static k8sApi;

  static before(): Promise<any> {
    
    const kc = new k8s.KubeConfig();
    kc.loadFromDefault();
    
    Suite.k8sApi = kc.makeApiClient(k8s.Core_v1Api);
    
    const namespace = {
      metadata: {
        name: 'test'
      }
    };
    
    return Suite.k8sApi.createNamespace(namespace).then(
      (response) => {
      
        // ... fire up some pods (kafka/zookeeper, microservice consumer/producer)
      }, (err) => {
      
        return Promise.reject(err);
      }
    );
  }

  @test
  integrationTest() {

    // send some message via kafka
    
    // consume and test expected events from kafka topic
  }

  static after(): Promise<any> {

    const k8sApi = Suite.k8sApi;
    Suite.k8sApi = null;

    // tear down pods and namespace

    const namespace = {
      metadata: {
        name: 'test'
      }
    };

    return k8sApi.deleteNamespace(namespace).then(
      (response) => {
      
        // ... 
      }, (err) => {
      
        return Promise.reject(err);
      }
    );
  }
}
{% endhighlight %}


{:.toc}
## Instance Lifecycle Hooks {% include support-all.html %}

`Testdeck` also supports instance level test lifecycle hooks, which also go by the names `before` and `after`. These
hooks are the equivalent to the `beforeEach` and `afterEach` test lifecycle hooks found in either of the supported test
frameworks.

{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';

import { NgLangParser } from '../src/NgLangParser';

@suite
class Suite {

  private cut: NgLangParser;

  before() {
  
    this.cut = new NgLangParser();
  }

  @test
  mustParseEmptyString() {

    this.cut.parse("");
  }

  after() {

    this.cut = null;
  }
}
{% endhighlight %}