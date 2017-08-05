---
title: "Use Data Providers"
date: 2017-08-07T19:59:31+01:00
draft: true
categories: ["unit-testing", "data-providers"]
tags: ["phpunit", "data-providers", "php"]
---
You might have a test that you want to run with multiple inputs. Rather than writing tests like so:

```php
<?php

class AdderTest extends \PHPUnit\Framework\TestCase
{    
    /**
     * @test 
     */
    public function it_adds_1_and_2_together() {
        self::assertSame(3, (new Adder)->add(1, 2));
    }
    
    /**
     * @test 
     */
    public function it_adds_100_and_45_together() {
        self::assertSame(145, (new Adder)->add(100, 45));
    }
    
    // etc etc
}
```

You can use something like [PHPUnit's Data Providers](https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.data-providers) to run a test multiple times with different inputs for each one:

```php
<?php

class AdderTest extends \PHPUnit\Framework\TestCase
{    
    /**
     * @test 
     * @dataProvider numbers 
     */
    public function it_adds_two_numbers_together(int $numberOne, int $numberTwo, int $expectedResult)
    {
        self::assertSame($expectedResult, (new Adder)->add($numberOne, $numberTwo));
    }
    
    public function numbers(): array
    {
        return [
            [1, 2, 3],
            [100, 45, 145],
            [0, 0, 0]
            // etc
        ];
    }
}
```

Now, our test function stays the same, but each item provided from the data provider will pass numbers into our test that we can use to test a variety of potential results.

