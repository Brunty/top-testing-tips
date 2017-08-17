---
title: "Add Better Messages To Assertions"
date: 2017-08-17T12:55:23+01:00
draft: false
categories: ["unit testing"]
tags: ["phpunit", "kahlan", "php", "assertions"]
---

If we had the following test of a repository class:

```php
/**
 * @test
 */
public function it_finds_a_task()
{
    $repository = new Repository;

    $task = $repository->findTask(1);

    self::assertInstanceOf(Task::class, $task);
}
```

If our implementation of the `findTask()` method didn't work as expected the test could fail as such:

```bash
F                                                                   1 / 1 (100%)

Time: 54 ms, Memory: 2.00MB

There was 1 failure:

1) RepositoryTest::it_finds_a_task
Failed asserting that null is an instance of class "Task".
```

Specifically, this line is what we'll be focusing on:

```bash
Failed asserting that null is an instance of class "Task".
```

It tells us the test failed because null wasn't equal to the class that we're expecting to get. But we can add a custom error message to our assertion to give us a more meaningful error if the test fails:

```php
/**
 * @test
 */
public function it_finds_a_task()
{
    $repository = new Repository;

    $id = 1;
    $task = $repository->findTask($id);

    self::assertInstanceOf(
        Task::class,
        $task,
        "Could not find a task with the id {$id} in the database"
    );
}
```

It's a small change, but can have a big impact. We've extracted the ID we're trying to find out into a variable so we can use it in the error message we want to display.

Now, if the test fails, we'll see:

```bash
F                                                                   1 / 1 (100%)

Time: 96 ms, Memory: 2.00MB

There was 1 failure:

1) RepositoryTest::it_finds_a_task
Could not find a task with the id 1 in the database
Failed asserting that null is an instance of class "Task".
```

Not only do we get the error that we had before we added our own custom message, but we also get the output of our own error message to aid in tracking down the problem that was encountered with the test.

What parameter the message is in the assertion will depend on the on the assertion used in [PHPUnit](http://phpunit.de).

The assertion method signatures will usually tell you. For example:

```php
/**
 * Asserts that a variable is of a given type.
 *
 * @param string $expected
 * @param mixed  $actual
 * @param string $message
 */
public static function assertInstanceOf($expected, $actual, $message = '')
``` 

