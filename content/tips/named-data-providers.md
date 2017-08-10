---
title: "Named Data Providers"
date: 2017-08-10T21:07:17+01:00
draft: false
categories: ["unit-testing", "data-providers", "naming"]
tags: ["phpunit", "data-providers", "php"]
series: ["The human stuff"]
---

In the [last tip posted](https://toptestingtips.com/tips/using-data-providers), I showed how data providers can help with providing different input to the same test.

Sometimes, data providers can feel a little difficult to work out what data is going where. Or if one fails, it can be trickier than you might like to debug exactly which one went wrong. Error messages aren't always the clearest of things.

In PHPUnit you can name your data providers, so that when one of them fails, the name of that set of data is included in the output for debugging purposes.

In the following data provider one of our results will not work - because 9000 + 1 is not 9002.

```php
    public function numbers(): array
    {
        return [
            [1, 2, 3],
            [100, 45, 145],
            [0, 0, 0],
            [1, 1, 2],
            [9000, 1, 9002]
        ];
    }
```

When we run PHPUnit for a test using this provider, we see the following:

```bash
PHPUnit 6.2.3 by Sebastian Bergmann and contributors.

....F                                                               5 / 5 (100%)

Time: 68 ms, Memory: 4.00MB

There was 1 failure:

1) App\Tests\AdderTest::it_adds_two_numbers_together with data set #4 (9000, 1, 9002)
Failed asserting that 9001 is identical to 9002.

/Users/brunty/code/toptestingtips/tests/AdderTest.php:16

FAILURES!
Tests: 5, Assertions: 5, Failures: 1.
```

Now, this is a fairly simple example so it's not the hardest data provider to debug. However, with anything more complex it can be hard to work out what data set you're dealing with. It's telling us that data set #4 isn't working. Which is the 5th item in our array _(0-indexed remember)_.

We can improve this by giving each of the sets of data a name like so:

```php
    public function numbers(): array
    {
        return [
            'one plus two'                => [1, 2, 3],
            'one hundred plus forty five' => [100, 45, 145],
            'zero plus zero'              => [0, 0, 0],
            'one plus one'                => [1, 1, 2],
            'nine thousand plus one'      => [9000, 1, 9002]
        ];
    }
```

Now when we run the test with this data provider, the output will tell us the name of the data that failed our test:

```bash
PHPUnit 6.2.3 by Sebastian Bergmann and contributors.

....F                                                               5 / 5 (100%)

Time: 97 ms, Memory: 4.00MB

There was 1 failure:

1) App\Tests\AdderTest::it_adds_two_numbers_together with data set "nine thousand plus one" (9000, 1, 9002)
Failed asserting that 9001 is identical to 9002.

/Users/brunty/code/toptestingtips/tests/AdderTest.php:16

FAILURES!
Tests: 5, Assertions: 5, Failures: 1.
```

It's a small change, but can make the difference when working on a project with lots of tests and data providers that are supplying a lot of data to what you're working with.
