---
title: Mutation Testing in Java with PITest
date: 2023-02-24
categories: [java, testing]
description: An overview of mutation testing and how to use PITest, a mutation testing library for Java, to measure the quality of test cases.
draft: true
---

## Why Do Mutation Testing

As developers, we are all hopefully aware of the importance of testing. Traditional testing methods involve running a series of test cases on a program's code to check if the code behaves as expected. However, it can be challenging to determine the effectiveness of test cases in detecting faults or defects in the program. This is where mutation testing comes in.

Taking this example class:
```java
public class WaterPot {
    private int temperature;

    public class WaterPot(int temperature) {
        this.temperature = temperature;
    }

    boolean isBoiling() {
        return this.temperature >= 100;
    }
}
```

We might write a few tests to check it's correctness.
```java
public class WaterPotTest {

    public void givenHighTemperature_shouldBeBoiling() {
        WaterPot waterPot = new WaterPot(1000);

        assertTrue(waterPot.isBoiling());
    }

    public void givenLowTemperature_shouldNotBeBoiling() {
        WaterPot waterPot = new WaterPot(10);

        assertFalse(waterPot.isBoiling());
    }
}
```

We run the tests and they work. We even check the coverage and it's 100%. Cool! We have a functioning method fully covered by tests.

However, one day someone (maybe you again) come back to the class and think "Why there's a greater than or equals? We can just check for it to be greater" and decide to update the class:
```java
public class WaterPot {
    private int temperature;

    public class WaterPot(int temperature) {
        this.temperature = temperature;
    }

    boolean isBoiling() {
        return this.temperature > 100;
    }
}
```

You launch the test suite and all tests passes. Cool, you simplified the code and everything still works.

But! The code was correct before, now you added a bug that will be quite hard to catch. No one notices and you push in production, after all the tests passed!

Now, this is a very simple example, but these things can happen. This is why I find mutation testing to be a useful tool to help catch these issues.

Mutation testing is a type of software testing that involves modifying small parts of a program's code, known as "mutations," to check whether the existing test cases can detect these changes. The goal of mutation testing is to measure the quality of a set of test cases by determining their effectiveness in detecting faults or defects in the program.

If the test cases can detect the mutations, then they are considered effective, and the program is considered to have a high level of fault tolerance. If the test cases fail to detect the mutations, then they are considered ineffective, and the program may need further testing or debugging.

In the above example, a mutation would be to modify `>=` into `>`. If all tests regarding that function passes, the mutation lives and the mutation testing fails. We can use this failure to notice that we didn't properly test that function.

## Introducing PITest

_PIT is a state of the art mutation testing system, providing gold standard test coverage for Java and the jvm. It's fast, scalable and integrates with modern test and build tooling._
Also, it's the only one I found for Java.

### Installation and Running
Installation instructions can be found on the [quickstart page](https://pitest.org/quickstart/). 
I'm using Gradle and to install it I simply modified my `build.gradle` file:
```groovy
plugins {
    ...
    id 'info.solidsoft.pitest' version '1.9.0'
}

...

test {
    ...
    systemProperty 'spring.test.constructor.autowire.mode', 'all' // Fixed some issues for me
}

pitest {
    threads = 8
    junit5PluginVersion = '1.0.0'
    jvmArgs = ['-Xmx1024m', '-Dspring.test.constructor.autowire.mode=all']
    exportLineCoverage = true
}
```

To start PIT, in the terminal run `gradle pitest`

## Results
Once PITest is done running it will output the results in the console. If you have more than two tests, it will be quite hard to comprehend. The actual results will be stored in `build/reports/pitest` in HTML format. You can open the `index.html` file in the browser to navigate the test results in a more readable way. 
Once opened you are going to see 3 metrics – line coverage, mutation coverage and test strength.

### Line Coverage
Line coverage is the measure of how many lines are actually covered by tests.
Nothing too fancy here.

### Mutation Coverage
Mutation coverage measures how many mutants were killed out of all mutants created. By going into a specific class we can see details on which mutants survived and which died.

### Test Strength
The test strength is basically the score of how many mutants got killed out of all the mutants that were created, excluding those that were not tested at all. 
This means that if a mutant survives because the method it mutated isn't tested, it won't count towards this metric.

## Possible Issues
Mutation testing is a very intensive operation, completing it may take hours. The huge amount of tests that are runned can create several problems.


By analyzing these results we can determine where our tests are lacking.

### Random Test Failures
You run your test suite and every test passes. You then run PIT, all seems fine but after a while it fails with the following message:

`* tests did not pass without mutation when calculating line coverage. Mutation testing requires a green suite.`

This basically means that some “normal” test failed. In my case it was due to having too many connections to the database opened. Postgres started rejecting new connections and this caused test failures.

There are various reason as to why too many connections were left open:

- I created too many SpringBoot `Contexts`. Context is cached, but it varies depending on the `MockBean` used. It means that if two classes wants to share context, but one has a Mocked bean and the other not, two contexts will be created.
To solve this, we need to discard cached contexts with the `@DirtiesContext()` annotation
    
- We have a connection pool too big. Update the `application.properties)` file to have a smaller size (I use HikariCP):

```java
spring.datasource.hikari.maximum-pool-size=2
spring.datasource.hikari.minimum-idle=1
```

### Infinite runtime

Launching all the tests can take hours to finish. From personal experience, it’s better to focus on a single package at a time rather than launching a million mutations on the whole project.

```groovy
pitest {
	...
  targetClasses = ['dev.alessiofranceschi.myproject.service.*']
}
```

## Further Readings
- [PITest docs](https://pitest.org/quickstart/)
- [Good old Baeldung guide](https://www.baeldung.com/java-mutation-testing-with-pitest)