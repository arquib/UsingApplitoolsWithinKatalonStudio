Using Applitools within Katalon Studio
===========

# What is this?

This is a simple [Katalon Studio](https://www.katalon.com/) project for demonstration purpose. You can check this out onto your PC and execute with your Katalon Studio.

This will show you how I managed to use [Applitools](https://applitools.com/) within Katalon Studio. Applitools is a visual testing and monitoring service. In this demo, a Test Case instanciates an Applitools' `Eyes` object and calls its methods. The `Eyes` object takes  screenshots of AUT and transfer images to the Applitools service where various testing/monitoring features are offered.

# Why I made this

I have experienced the feature of [taking screenshot by Katalon keyword](https://docs.katalon.com/display/KD/%5BWebUI%5D+Take+Screenshot) and found that taking screenshots as PNG files is just a starting point. I have to view all the images taken, think about them, and find problems if any --- these tasks are difficult enough without appropriate tool's support.

One day I read a post in the [Katalon Community Forum](https://forum.katalon.com/discussions):

- [Applitools | Anyone had success in setting up within Katalon](https://forum.katalon.com/discussion/5628/applitools-anyone-had-success-in-setting-up-within-katalon)

Until then I was not aware of [Applitools](https://applitools.com/) then and got interested in the idea of using Applitools within [Katalon Studio](https://www.katalon.com/).

After a dozen hours of hacking, I have created a Katalon project here. In this demo project, a Katalon Test Case invokes Applitools Eyes API. This demo works for me.

# How this project is designed

Applitools has a published article
- ['Walkthrough : Running a visual test and reviewing test results']( https://applitools.com/docs/topics/overview/walkthough-example.html)

This article gives me an example of writing and running a visual test using native Selenium WebDriver combined with Applitools' Eyes SDK. Here I interpreted 'natve Selenium WebDriver' to 'Katalon Studio'. I mimicked the published example. I modifed it slightly to form a Katalon Test Case in Groovy.

# How to run the Demo

1. You need to register yourself to [Applitools](https://applitools.com/users/register) and obtain your Applitools API Key.
2. You want to create an environment variable `APPLITOOLS_API_KEY` to set the API Key as its value.
3. git clone [this project](https://github.com/kazurayam/UsingApplitoolsWithinKatalonStudio) to your PC. Start your Katalon Studio. Open the downloaded project.
4. load "Test Cases/test01" and Run it with any browser you have. I used Firefox.
5. Ensure the Test Case ran successfully
6. With browser you want to sign in [Applitools Eyes dashbord](https://applitools.com/users/login) with your account.
7. In the [https://eys.applitools.com/app/test-results]( https://eyes.applitools.com/app/test-results/) page you would find a set of screenshots transfered to Applitools: ![like this](
    https://github.com/kazurayam/UsingApplitoolsWithinKatalonStudio/blob/master/docs/images/applitools_eys.PNG?raw=true)

# Points to be remarked

## External dependencies

Katalon Studio has its own set of dependencies, and Applitools has its own. Nobody takes responsibility of making these two sets consistent. I think this would be the point which makes my hack operational or not.

1. Applitools requires a lot of external libiraries. See ['Walkthrough : Running a visual test and reviewing test results' Getting SDK]( https://applitools.com/docs/topics/overview/walkthough-example.html) to see how to download the SDK Zip. The Java SDK contains 63 jar files.
2. I registered [16 jar files](https://github.com/kazurayam/UsingApplitoolsWithinKatalonStudio/tree/master/Drivers) out of the Applitools SDK into the Katalon Studio's project, as [described in the Katalon document](https://docs.katalon.com/display/KD/External+Libraries).
3. I have chosen 16 jar files by try-and-error steps. When I encounter a java.lang.NoClassDeffFoundErr, I looked for an appropriate jar file in the downloaded SDK, and put it into Katalon Studio. Therefore you are quite likely to encounter more of java.lang.NoClassDeffFoundError.

## how to code

As Test Case  [test01](https://github.com/kazurayam/UsingApplitoolsWithinKatalonStudio/blob/master/Scripts/test01/Script1523260020936.groovy) shows, the order of following steps is important. This works:
```
WebUI.openBrowser('')
WebDriver innerDriver = DF.getWebDriver()
Eyes eyes = createEyes()
eyes.open(innerDriver, GlobalVariable.appName,
     GlobalVariable.testName, viewportSize)
```


But this does not work:
```
WebDriver innerDriver = DF.getWebDriver()
WebUI.openBrowser('')
```
