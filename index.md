---
layout: frontpage
section: frontpage
role: index
label: Home
order: 0
---


<div class="tile is-anchestor">
  <div class="tile">
    <div class="tile is-vertical is-6">
      <div class="tile is-parent">
        <div class="tile is-child">
          <div class="title">What is testdeck?</div>
          <div class="content">
            <p>testdeck is a decorator based wrapper around your favourite <em>JavaScript</em> test framework.</p>
            <p>With testdeck, you can write your tests object-orientedly, effectfully making use of 
            <a href="/pages/guide/inheritance">inheritance</a> to encapsulate test suite behaviour.</p>
            <p>testdeck also supports browser based testing using for example <em>Karma</em>.</p>
            <p>testdeck also makes no assumptions on the 
            <a href="/pages/guide/setup#assertionexpectation-frameworks">assertion/expectation frameworks</a>
            that you want to use.</p>
            <p>testdeck is also the official successor of <em>mocha-typescript</em>, so if you have been using that before,
            you can now simply switch to <em>@testdeck/mocha</em> and your existing tests will work right out of the box.</p>
          </div>
        </div>
      </div>
      <div class="tile is-parent">
        <div class="tile is-child">
          <div class="content">
            {% include sample_code/mocha-simple-suite.html %}
          </div>
        </div>
      </div>
    </div>
    <div class="tile is-vertical is-6">
      <div class="tile is-parent">
        <div class="tile is-child">
          <div class="title">Not Limited to Just <em>TypeScript</em></div>
          <div class="content">
            <p>While testdeck was written using <em>TypeScript</em>, it can be used with also Babel or any other transpiled
            language that features support for <em>JavaScript</em> decorators.</p>
            <p>We are currently in the process of compiling multiple example projects, called seeds, that you can find
            on <a href="https://github.com/search?q=testdeck-*-seed">GitHub</a>.</p>
            <p>So check these out to find out more about how to integrate <em>testdeck</em> with for example <em>Babel</em>.</p>
            <p>And if you need more examples, feel free to <a href="/pages/contributing/reporting#feature-requests">issue a feature request</a>
            or even provide us with an example of your own.</p>
          </div>
        </div>
      </div>
      <div class="tile is-parent">
        <div class="tile is-child">
          <div class="title">Supported Test Frameworks</div>
          <div class="content">
            <p>testdeck already supports the most used test frameworks.</p>
            <p>To find out more, see the <a href="/pages/guide/setup">setup guide</a> or use one of the links below.</p>
          </div>
          <div class="columns">
            <div class="column">
              <figure class="image is-128x128">
                <a href="/pages/guide/setup#testdeckmocha">
                  <img class="vendor-logo" src="/assets/img/logo-mocha.svg">
                </a>
              </figure>
            </div>
            <div class="column">
              <figure class="image is-128x128">
                <a href="/pages/guide/setup#testdeckjasmine">
                  <img class="vendor-logo" src="/assets/img/logo-jasmine.svg">
                </a>
              </figure>
            </div>
            <div class="column">
              <figure class="image is-128x128">
                <a href="/pages/guide/setup#testdeckjest">
                  <img class="vendor-logo" src="/assets/img/logo-jest.svg">
                </a>
              </figure>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
