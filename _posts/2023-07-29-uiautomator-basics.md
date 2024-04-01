---
layout: post
title:  "UI Automator Basics"
date:   2023-07-29 23:11:02 +0800
---

## Why use UI Automator

- **Cross-app testing**: UI Automator is designed for cross-app testing, meaning it can interact with elements across different Android apps. This makes it suitable for scenarios where you need to test interactions between multiple apps or perform actions like taking screenshots, accessing notifications, or interacting with system dialogs. Espresso, on the other hand, is primarily focused on testing a single app's user interface interactions and doesn't provide direct support for interactions with elements outside the app under test.

- **System-level interactions**: UI Automator is capable of testing system-level interactions like turning on/off Wi-Fi, changing device settings, rotating the screen, or simulating hardware button presses. These capabilities are not directly available in Espresso, as it focuses more on testing within the app's user interface.

- **Testing scenarios involving WebView**: If your Android app contains WebView components that display web content, UI Automator can be useful for testing interactions within the WebView. While Espresso does provide limited support for WebView testing, UI Automator can offer more comprehensive control and access to the content within the WebView.

- **Wide device and OS version compatibility**: UI Automator generally offers better compatibility with a wide range of Android devices and OS versions. It is designed to work on Android 4.3 (API level 18) and higher, allowing you to test on older devices that Espresso might not support. Espresso, being part of the AndroidX Test library, has a minimum API level requirement of 14, which may not cover some older devices.

## How to get started

To get started with UI Automator in Android Studio, you need to set up a new project and configure it to use the UI Automator testing framework. Here's a step-by-step guide to help you get started:

1. **Set up Android Studio**: Ensure that you have Android Studio installed on your machine. If you don't have it yet, download and install the latest version from the official Android Studio website.

2. **Create a new Android test project**: Open Android Studio and create a new Android project or open an existing one where you want to add UI Automator tests. To create a new project, go to "File" > "New" > "New Project."

3. **Add UI Automator dependencies**: In your project's `build.gradle` file (located in the app module), add the necessary dependencies for UI Automator:

```gradle
dependencies {
    // Other dependencies...
    androidTestImplementation 'androidx.test.uiautomator:uiautomator:2.2.0'
}
```

4. **Create a UI Automator test class**: Now, it's time to create a UI Automator test class. This class will contain the test cases that interact with your app's user interface. Create a new Java file inside the `androidTest` package of your app module and give it a meaningful name, such as "MyAppUITest.java."

5. **Write your UI Automator tests**: Inside the test class, you can start writing your UI Automator tests. UI Automator tests are based on JUnit, and you'll use the `UiDevice` class to interact with the device and perform actions on the user interface elements.

Here's a simple example of a UI Automator test:

```java
import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.uiautomator.UiDevice;
import androidx.test.uiautomator.UiObject;
import androidx.test.uiautomator.UiSelector;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;

import static androidx.test.platform.app.InstrumentationRegistry.getInstrumentation;
import static org.junit.Assert.assertTrue;

@RunWith(AndroidJUnit4.class)
public class MyAppUITest {

    private UiDevice device;

    @Before
    public void setUp() {
        // Initialize UiDevice instance
        device = UiDevice.getInstance(getInstrumentation());
    }

    @Test
    public void testButtonIsClickable() {
        // Find a button with the text "Click Me"
        UiObject button = device.findObject(new UiSelector().text("Click Me"));

        // Perform an action - click the button
        button.click();

        // Assert the button click action was successful
        assertTrue("Button should be clickable", button.isClickable());
    }
}
```

6. **Run the UI Automator test**: Connect your Android device to your computer or use an emulator. Ensure that your device is connected and the app you want to test is installed on the device. Right-click on your test class and select "Run 'MyAppUITest'" (or any other name you used for your test class). This will start the test execution, and you'll see the test results in the "Run" tab in Android Studio.

## Introduction to UiAutomator library functions

UI Automator is a testing framework provided by Android for automated functional testing of Android applications. It allows developers to write tests that interact with the user interface elements of an Android app, such as buttons, text fields, and dialogs. The UI Automator library provides various functions to find and interact with these elements on the device. Here's an introduction to some of the essential UI Automator library functions:

- **UiDevice**: The UiDevice class is the entry point to the UI Automator library. It represents the device under test and provides methods to perform various device-level operations, such as pressing hardware buttons, rotating the screen, and waking up the device.

```java
UiDevice uiDevice = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation());
uiDevice.pressBack(); // Press the Back button
uiDevice.setOrientationLeft(); // Rotate the screen to the left
```

- **UiObject**: UiObject represents a UI element on the device, such as a button or a text field. It allows you to find elements using various selectors and interact with them by performing actions like clicking, typing text, or getting element properties.

```java
UiObject button = uiDevice.findObject(new UiSelector().text("Click Me"));
button.click(); // Click the button
button.setText("Hello, UI Automator!"); // Set text on the element
String buttonText = button.getText(); // Get the text of the button
```

- **UiSelector**: UiSelector is a class used to specify the properties of the UI element you want to interact with. You can use various methods to set different attributes like text, resource ID, content description, etc., to uniquely identify the element.

```java
UiSelector buttonSelector = new UiSelector().resourceId("com.example.app:id/click_button");
UiObject button = uiDevice.findObject(buttonSelector);
button.click(); // Click the button with the specified resource ID
```

- **UiScrollable**: UiScrollable is used to interact with scrollable elements, such as lists or scroll views. It provides methods to scroll up, down, left, or right and helps you access elements that are not currently visible on the screen.

```java
UiScrollable scrollableList = new UiScrollable(new UiSelector().scrollable(true));
scrollableList.scrollForward(); // Scroll forward in the list
scrollableList.scrollBackward(); // Scroll backward in the list
```

- **UiCollection**: UiCollection is used to interact with a group of elements that share the same UI class or other properties. It allows you to perform actions on individual elements within the collection.

```json
UiCollection listView

 = new UiCollection(new UiSelector().className("android.widget.ListView"));
int count = listView.getChildCount(); // Get the number of items in the ListView
UiObject item = listView.getChild(new UiSelector().index(3)); // Get the 4th item in the ListView
```

These are just a few of the essential functions provided by the UI Automator library. There are many more methods and classes available, allowing you to create comprehensive and powerful UI tests for your Android applications. When using UI Automator, make sure to import the necessary classes and use the appropriate methods to find and interact with the UI elements effectively.

## Real World Example

Let's create a simple UI Automator test example that interacts with an Android app. For this example, let's assume we have an app with a login screen containing two EditText fields for username and password, and a login button. Our test will perform the following steps:

1. Launch the app.
2. Enter valid username and password.
3. Click the login button.
4. Verify that the login is successful.

Assuming the app package name is com.example.myapp and the Activity name is LoginActivity, here's the UI Automator test example:


```java
import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.platform.app.InstrumentationRegistry;
import androidx.test.uiautomator.By;
import androidx.test.uiautomator.UiDevice;
import androidx.test.uiautomator.UiObject;
import androidx.test.uiautomator.UiObjectNotFoundException;
import androidx.test.uiautomator.UiSelector;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;

import static org.junit.Assert.assertTrue;

@RunWith(AndroidJUnit4.class)
public class MyLoginTest {

    private UiDevice device;

    @Before
    public void setUp() {
        device = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation());
        device.pressHome();
    }

    @Test
    public void testLogin() throws UiObjectNotFoundException {
        // Launch the app
        device.findObject(By.text("MyApp")).click();

        // Wait for the login screen to appear
        device.waitForIdle();

        // Find username and password EditText fields
        UiObject usernameField = device.findObject(new UiSelector().resourceId("com.example.myapp:id/edit_username"));
        UiObject passwordField = device.findObject(new UiSelector().resourceId("com.example.myapp:id/edit_password"));

        // Enter username and password
        usernameField.setText("testuser");
        passwordField.setText("password123");

        // Find and click the login button
        UiObject loginButton = device.findObject(new UiSelector().resourceId("com.example.myapp:id/btn_login"));
        loginButton.click();

        // Wait for the login process to complete
        device.waitForIdle();

        // Verify that login is successful
        UiObject loggedInMessage = device.findObject(new UiSelector().text("Welcome, testuser!"));
        assertTrue("Login was not successful", loggedInMessage.exists());
    }
}
```

