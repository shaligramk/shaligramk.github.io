---
layout: post
title:  "Up and Running with Selenium"
date:   2023-07-28 23:11:02 +0800
---
### Up and Running with Selenium

This post covers a basic implementation of a Selenium testing framework from the ground up using python. It includes example code, in addition to a number of links that were useful when developing my framework.

## Design
When you're building out a testing framework, it's important to think about how it's going to be used and who is going to be using it. Here are some things to consider:

  - Tests should be easy to debug
  - Tests should be hermetic
  - Tests should run fast
  - Tests should integrate well with developer workflow
  - Tests should integrate with existing CI/CD Build Systems
  - Tests should be easy to write and maintain

***

## A basic test suite

One of the libraries I have used for a number of testing projects is [pytest](https://docs.pytest.org/en/latest/). It is the foundation of many testing frameworks and it offers:

* Fixtures (setup and teardown)
* No boilerplate, no required api
* Test collection and discovery
* xUnit report output

***

## Installation
First, let's install the dependencies below. You can use a
[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/)
(preferred), but you can also install the packages system-wide.

* `pytest`
* `selenium`
* `simplejson`

***

## Setup

Now, fire up your favorite text editor, and let's create an base test case with fixtures we can use for all test cases:

    from selenium import webdriver

    class SeleniumBaseTestCase(object):

        def setup(self):
            self.driver = webdriver.Firefox()
            self.driver.implicitly_wait(10)

        def teardown(self):
            self.driver.quit()

Fixtures are wrappers around tests that are run before and after the test. In the above example, we startup a new firefox browser for each test and quit the browser when the test has finished. Even if the test fails, the teardown method will still be called.

***

## Writing your test

Here is an example of what your test might look like:

        import pytest


        @pytest.mark.usefixtures('driver')
        class TestGuineaPig(object):

            def test_link(self, driver):
            """
            Verify page title change when link clicked
            :return: None
            """
            driver.get('https://google.com/')
            driver.find_element_by_id("header").click()

            title = "Google"
            assert title == driver.title

***

## Page Object Model
Page Object Model - A page object model is a design pattern which allows you to maintain selectors and page actions in one place rather than individual tests. Its main purpose is to enhance test maintenance and reducing code duplication.

***

## Selectors

The most difficult element of testing is writing robust selectors. Brittle selectors can break whenever slight changes occur in the DOM. Selectors should be:

* Unique
* Descriptive
* Short

CSS selectors are preferred over XPath. They are faster (especially in IE) and more readable. Here's a [blog post and video](http://sauceio.com/index.php/2011/05/why-css-locators-are-the-way-to-go-vs-xpath/) from Santi of Sauce Labs about the advantages of CSS selectors. Not covered in Santi's post is the javascript-xpath
library, which should yield significantly better XPath performance. This tip came from Dr. Wenhua Wang. You should be able to find his presentation with details on javascript-xpath
at his [Meetup event page](http://www.meetup.com/seleniumsanfrancisco/events/101087592/).

And here's a [comparison of CSS and XPath syntax](http://ejohn.org/blog/xpath-css-selectors/) from John Resig.
Targeting sibling or parent elements are two situations where you cannot use CSS and need to use XPath.

***

## Sauce Labs

Sauce Labs allows you to run your tests in the cloud on
[over 150 platform/browser combinations](https://saucelabs.com/docs/platforms).

To use Sauce Labs, we just need to modify our setup fixture to create a remote webdriver instead of the Firefox
one we were using previously. Let's modify our module and test level fixtures:

    def setup():
        config_json = """
        {
            "endpoint": "http://google.com",
            "timeout": 10,
            "use_sauce": true,
            "sauce_username": "my_username",
            "sauce_access_key": "accesske-y012-3456-789a-bcdef0123456",
            "sauce_browser": "CHROME"
        }
        """
        global config
        config = json.loads(config_json)

    class SeleniumBaseTestCase(object):

        def setup(self):
            if config['use_sauce']:
                desired_capabilities = getattr(webdriver.DesiredCapabilities, config['sauce_browser'])
                self.driver = webdriver.Remote(
                    desired_capabilities=desired_capabilities,
                    command_executor="http://%s:%s@ondemand.saucelabs.com:80/wd/hub" % (
                        config['sauce_username'], config['sauce_access_key']
                    )
                )
            else:
                self.driver = webdriver.Firefox()

            self.driver.implicitly_wait(config['timeout'])

***

## Cross-browser testing

There are some advanced ways to do cross-browser testing, using a custom nose plugin and method decorators, but it's
more trouble than it's worth. It's easier to just run the suite multiple times with a different configuration file. In
Jenkins, use multiple jobs with different configuration files to make it easier to track test results. This way you can
also fail a build on Firefox test failures, but let the build continue even if tests fail on Internet Explorer.
Personally, I have had a lot of trouble getting tests to be 100% stable on anything but the browser they were written
for once you start writing tests that interact with a lot of JavaScript and navigate between multiple pages.

Look at [Selenium's own Jenkins instance](http://ci.seleniumhq.org:8080/) to understand how browsers may
behave differently.

***

## Sauce Connect
You can even test against local, non-internet facing, environments using a
[Sauce Connect tunnel](https://saucelabs.com/docs/connect). They are incredibly easy to use, just execute the .jar
with your credentials. Some things to watch out for when using Sauce Connect:

* Tunnels get stale, and should be refreshed daily using a scheduled job. They can also die unpredictably.
See [Keeping Sauce Connect Fresh](http://support.saucelabs.com/entries/21068387-Keeping-Sauce-Connect-fresh).
* All traffic during Sauce tests go through the tunnel, not just traffic to the local environment. This can slow
things down quite a bit.
* Only one tunnel can be active per account. If you need to tunnel to multiple environments,
you should create [sub-accounts](https://saucelabs.com/docs/subaccounts) and manage credentials accordingly.

***

## Sauce API

Sauce Labs has a dashboard where you can view tests. If you want the dashboard to contain any meaningful data, do the following:

* Set the `name` parameter in `desired_capabilities` in the setup. Unfortunately a setup has no good way of
knowing what method called it. I like to use `'%s.%s.?' % (__module__, __class__.__name__)`.
This would result in file_name.ClassName.?
* Set the `build` parameter in `desired_capabilities`. Jenkins provides a
[$BUILD_TAG parameter](https://wiki.jenkins-ci.org/display/JENKINS/Building+a+software+project#Buildingasoftwareproject-JenkinsSetEnvironmentVariables)
that contains the job name and build number so you know which build the test originated from.
* Use Sauce Labs' API to [update the job](https://saucelabs.com/docs/rest#resources/update)
after the test has completed. I parse the xUnit report and use the API to complete the test name
and update the pass/fail status.
See [job_update_sauce.py](https://github.com/danielhochman/basic_selenium_framework/blob/master/basic_selenium_framework/job_update_sauce.py).

***

## Jenkins integration

[Jenkins](http://jenkins-ci.org/) is a great tool for automating the deployment process.
Post-commit [git hooks](http://git-scm.com/book/ch7-3.html) can trigger a build in Jenkins
every time a developer pushes code. Tests should run as part the build.

Jenkins also supports test reporting on the front-end from an xUnit test report. A nice graph shows pass, fail, and test volume history. Each test is visible from Jenkins built-in test browser.

***

## Headless testing

The [`PyVirtualDisplay`](https://pypi.python.org/pypi/PyVirtualDisplay)
module allows you to wrap your program in `Xvfb` (a virtual X display). This is useful for running tests on your CI server (without Sauce), or locally if you don't want a ton of browser windows taking over your display. See [this post](http://coreygoldberg.blogspot.com/2011/06/python-headless-selenium-webdriver.html) from Corey Goldberg for more details, then add it to your test runner.

***

## Coverage

There is always this question on how to E2E Testing needs to be done as compared to other types of testing (manual, unit, integration,etc). Despite the power and flexibility of Selenium, I think it's important to stay as close to the metal as possible. If Selenium tests are written with the goal of providing significant coverage, the resulting suite will be brittle. Unit testing should be the primary source of coverage.

An additional strategy is to appoint someone to decide where tests for newly discovered bugs should be written. Closing out a bug should require that a test exists somewhere for the bug.

***
## Going Further!

Ask a question on the [user group](https://groups.google.com/forum/#!forum/selenium-users),
in the IRC channel (#selenium on Freenode),
or attend a [Meetup](http://selenium.meetup.com/).

All of the above resources were invaluable to me when learning about Selenium,
as well as developing and debugging my framework.