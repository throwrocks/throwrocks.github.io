---
layout: page
comments: false
title: Test the API with a Unit Test
permalink: fm-invoices-unit-test-api.html
---

It's time to test our API and APIResponse classes. To do this, we're going to write some Unit Tests using the Roboelectric library.

>Robolectric is a unit test framework that de-fangs the Android SDK jar so you can test-drive the development of your Android app.

We're using this framework because Unit Tests are meant to run in isolation. That means we shouldn't have to run the entire Android app just to test one class. However, since our code has dependencies on the Android SDK, the unit test will fail when they can't find the required objects. For the unit test to work, we would need to "mock" the external dependencies.

>Mock is a method/object that simulates the behavior of a real method/object in controlled ways.
> [Source](http://stackoverflow.com/questions/2665812/what-is-mocking)

Roboelectric takes care of mocking the Android objects by referring our calls to shadow objects that simulate them. 

### Add Robolectric

1. Open the /app/build/build.gradle file

2. Add the following line `testCompile "org.robolectric:robolectric:3.2.2"`

3. Click on *Sync Now*

![Add Roboelectric](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_01_add_roboelectric.png)

### Rename