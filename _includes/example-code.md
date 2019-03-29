{% highlight TypeScript linenos %}
import { suite, test } from '@testdeck/mocha';
import * as chai from 'chai';

@suite
class TestSuite {
  @test
  someTest() {
    chai.assertTrue(false);
  }
}
{% endhighlight %}
