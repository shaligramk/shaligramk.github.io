---
layout: page
title: Up and Running with Selenium
---

## Design
When you're building out a testing framework, it's important to think about how it's going to be used and who is going to be using it. Here are some things to consider:

  - Tests should be easy to debug
  - Tests should be hermetic
  - Tests should run fast
  - Tests should integrate well with developer workflow
  - Tests should integrate with existing CI/CD Build Systems
  - Tests should be easy to write and maintain

## A basic test suite

One of the libraries I have used for a number of testing projects is [pytest](https://docs.pytest.org/en/latest/). It is the foundation of many testing frameworks and it offers:

* Fixtures (setup and teardown)
* No boilerplate, no required api
* Test collection and discovery
* xUnit report output

## Installation
First, let's install the dependencies below. You can use a
[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/)
(preferred), but you can also install the packages system-wide.

* `pytest`
* `selenium`
* `simplejson`

## Setup

Now, fire up your favorite text editor, and let's create an base test case with fixtures we can use for all test cases:

    :::python
    from selenium import webdriver

    class SeleniumBaseTestCase(object):

        def setup(self):
            self.driver = webdriver.Firefox()
            self.driver.implicitly_wait(10)

        def teardown(self):
            self.driver.quit()

Fixtures are wrappers around tests that are run before and after the test. In the above example, we startup a new firefox browser for each test and quit the browser when the test has finished. Even if the test fails, the teardown method will still be called.

## Writing your test


## Page Object Model
Page Object Model - A page object model allows you to maintain selectors and page actions in one place rather than individual tests.

