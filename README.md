gesture [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Gesture-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/3326)
=======

detects gestures on Android with listener and RxJava Observable. 

This project is a fork of [better-gesture-detector](https://github.com/Polidea/better-gesture-detector) by [Polidea](https://github.com/Polidea).

JavaDoc: http://pwittchen.github.io/gesture/RxJava2.x

| Current Branch | Branch  | Artifact Id | Build Status  | Maven Central |
|:--------------:|:-------:|:-----------:|:-------------:|:-------------:|
| | [`RxJava1.x`](https://github.com/pwittchen/gesture/tree/RxJava1.x) | `gesture` | [![Build Status for RxJava1.x](https://travis-ci.org/pwittchen/gesture.svg?branch=RxJava1.x)](https://travis-ci.org/pwittchen/gesture) | ![Maven Central](https://img.shields.io/maven-central/v/com.github.pwittchen/gesture.svg?style=flat) |
| :ballot_box_with_check: | [`RxJava2.x`](https://github.com/pwittchen/gesture/tree/RxJava2.x) | `gesture-rx2` | [![Build Status for RxJava2.x](https://travis-ci.org/pwittchen/gesture.svg?branch=RxJava2.x)](https://travis-ci.org/pwittchen/gesture) | ![Maven Central](https://img.shields.io/maven-central/v/com.github.pwittchen/gesture-rx2.svg?style=flat) |

Contents
--------
- [Usage](#usage)
  - [Imperative way - Listener](#imperative-way---listener)
  - [Reactive way - RxJava](#reactive-way---rxjava)
- [Examples](#examples)
- [Download](#download)
- [Tests](#tests)
- [Code style](#code-style)
- [Static code analysis](#static-code-analysis)
- [Credits](#credits)
- [License](#license)

Usage
-----

### Imperative way - Listener

**Step 1**: Create `Gesture` attribute in the `Activity`:

```java
private Gesture gesture;
```

**Step 2**: Initialize `Gesture` object and add `GestureListner`:

```java
gesture = new Gesture();

gesture.addListener(new GestureListener() {
  @Override public void onPress(MotionEvent motionEvent) {
    textView.setText("press");
  }

  @Override public void onTap(MotionEvent motionEvent) {
    textView.setText("tap");
  }

  @Override public void onDrag(MotionEvent motionEvent) {
    textView.setText("drag");
  }

  @Override public void onMove(MotionEvent motionEvent) {
    textView.setText("move");
  }

  @Override public void onRelease(MotionEvent motionEvent) {
    textView.setText("release");
  }

  @Override public void onLongPress(MotionEvent motionEvent) {
    textView.setText("longpress");
  }

  @Override public void onMultiTap(MotionEvent motionEvent, int clicks) {
    textView.setText("multitap [" + clicks + "]");
  }
});
```

**Step 3**: Override `dispatchTouchEvent(MotionEvent)` method:

```java
@Override public boolean dispatchTouchEvent(MotionEvent event) {
  gesture.dispatchTouchEvent(event);
  return super.dispatchTouchEvent(event);
}
```

### Reactive way - RxJava

**Step 1**: Create `Gesture` attribute and `Subscription` in the `Activity`:

```java
private Gesture gesture;
private Disposable subscription;
```

**Step 2**: Initialize `Gesture` object and subscribe RxJava `Observable`:

```java
gesture = new Gesture();

subscription = gesture.observe()
  .subscribeOn(Schedulers.computation())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe(event -> {
    textView.setText(event.toString());
  });
```

`GestureEvent` is an enum with the following API:

```java
public enum GestureEvent {
  ON_PRESS,
  ON_TAP,
  ON_DRAG,
  ON_MOVE,
  ON_RELEASE,
  ON_LONG_PRESS,
  ON_MULTI_TAP;

  int getClicks();
  GestureEvent withClicks(int clicks);
}
```

**Step 3**: Override `dispatchTouchEvent(MotionEvent)` method:

```java
@Override public boolean dispatchTouchEvent(MotionEvent event) {
  gesture.dispatchTouchEvent(event);
  return super.dispatchTouchEvent(event);
}
```

**Step 4**: Don't forget to dispose subscription when it's no longer needed:

```java
@Override protected void onPause() {
  super.onPause();
  if (subscription != null && !subscription.isDisposed()) {
    subscription.dispose();
  }
}
```

Examples
--------

Exemplary application is located in `app` directory of this repository.

If you would like to see sample app in Kotlin, check `app-kotlin` directory.

Download
--------

latest version:  ![Maven Central](https://img.shields.io/maven-central/v/com.github.pwittchen/gesture-rx2.svg?style=flat)

replace `x.y.z` with the latest version

You can depend on library through Maven:

```xml
<dependency>
    <groupId>com.github.pwittchen</groupId>
    <artifactId>gesture-rx2</artifactId>
    <version>x.y.z</version>
</dependency>
```

or through Gradle:

```groovy
dependencies {
  compile 'com.github.pwittchen:gesture-rx2:x.y.z'
}
```

Tests
-----

To execute unit tests run:

```
./gradlew test
```

Code style
----------

Code style used in the project is called `SquareAndroid` from Java Code Styles repository by Square available at: https://github.com/square/java-code-styles.

Static code analysis
--------------------

Static code analysis runs Checkstyle, FindBugs, PMD and Lint. It can be executed with command:

 ```
 ./gradlew check
 ```

Reports from analysis are generated in `library/build/reports/` directory.

Credits
-------

Initial code base for this project is provided by [Polidea](https://github.com/Polidea).

License
-------

    Copyright 2012 Polidea
    Copyright 2016 Piotr Wittchen

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
