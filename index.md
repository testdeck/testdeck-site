---
layout: frontpage
section: frontpage
role: index
label: Home
order: 0
---

<div class="tile is-ancestor">
  <div class="tile is-parent">
    <div class="tile is-child is-4">
      <img src="/assets/img/logo-square.svg">
    </div>
    <div class="tile is-child is-8">
      <div class="content">
{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import * as chai from 'chai';

@suite
class TestSuite {

  @test
  someTest() {

    chai.assert.isOk(false);
  }
}
{% endhighlight %}
      </div>
    </div>
  </div>
</div>

<div class="columns testdeck is-centered">
  <div class="column">
    <figure class="image is-128x128">
      <a href="https://mochajs.org" target="_blank">
        <img class="vendor-logo" src="/assets/img/logo-mocha.svg">
      </a>
    </figure>
  </div>
  <div class="column">
    <figure class="image is-128x128">
      <a href="https://jasmine.github.io" target="_blank">
        <img class="vendor-logo" src="/assets/img/logo-jasmine.svg">
      </a>
    </figure>
  </div>
  <div class="column">
    <figure class="image is-128x128">
      <a href="https://jestjs.io" target="_blank">
        <img class="vendor-logo" src="/assets/img/logo-jest.svg">
      </a>
    </figure>
  </div>
</div>

