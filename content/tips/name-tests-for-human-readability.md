---
title: "Name Tests For Human Readability"
date: 2017-08-04T20:59:11+01:00
draft: false
author: Brunty
categories: ["unit-testing"]
tags: ["phpunit", "kahlan", "php"]
series: ["The human stuff"]
---
Consider the following class:

```php
<?php

class Adder
{
    public function add(int $a, int $b): int
    {
        return $a + $b;
    }
}
```

When writing a test for this, you may write a test method like so:

```php
<?php

class AdderTest extends \PHPUnit\Framework\TestCase
{    
    public function testAdd() {
        self::assertSame(3, (new Adder)->add(1, 2));
    }
}
```

While technically this works, it isn't really very descriptive of what's going on - it *appears* to just prepend the word `test` to the name of the method that's being called. Which by default, is how a tool such as [PHPUnit](https://phpunit.de) recognises that this is a method it should run as a test.

A better way to write it could be:

```php
<?php

class AdderTest extends \PHPUnit\Framework\TestCase
{   
    /**
     * @test
     */
    public function it_adds_two_numbers_together() {
        self::assertSame(3, (new Adder)->add(1, 2));
    }
}
```

The test method is much more descriptive of what it's testing now. It's more descriptive of the behaviour of the system under test. It's personal preference for some, but I feel it can make things a lot more readable.

It doesn't prepend the word `test` to indicate that it's a test, it uses PHP annotations (`/** */`) to indicate that it's a test via `@test`.

Another way to write this, would be with a `describe-it` style tool, such as [Kahlan](http://kahlan.github.io/docs/):

```php
<?php

describe('Adder', function () {
    it('adds two numbers together', function () {
        expect((new Adder)->add(1, 2))->toBe(3);
    });
});
```


