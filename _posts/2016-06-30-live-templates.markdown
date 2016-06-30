---
layout: post
title:  "Live Templates in Android Studio"
date:   2016-06-30 12:16:14 +0100
categories: androidstudio kotlin
---

Live templates allow you to quickly add text to the current file. They are context
aware so that only relevant templates for the current cursor location are shown.

You can insert the template into your file by writing the templates name, or by pressing
⌘+j to bring up the live template context menu, the you can search for the template by name.

![live templates example]({{ site.baseurl }}/img/template_example.gif)

## How to add?

- Copy the template from below
- Go to preferences (⌘+,) > Editor > Live Templates.
- Select the `Kotlin` section.
- Paste
- :moneybag:

## Result block for service call

Use this within the completion block on a service call that takes a Result.

```xml
<template name="serresult" value="result -&gt;&#10;&#10;result.success?.let {&#10;&#10;}&#10;&#10;result.failure?.let {&#10;&#10;}" description="service results block" toReformat="true" toShortenFQNames="true">
  <context>
    <option name="KOTLIN_EXPRESSION" value="true" />
  </context>
</template>
```

### Example output
```kotlin
result ->
  result.success?.let {
  }

  result.failure?.let {
  }
```

## Mock var for unit test
```xml
<template name="mock" value="@Mock&#10;lateinit var $NAME$: $TYPE$" description="Mock var" toReformat="false" toShortenFQNames="true">
  <variable name="NAME" expression="" defaultValue="" alwaysStopAt="true" />
  <variable name="TYPE" expression="" defaultValue="" alwaysStopAt="true" />
  <context>
    <option name="KOTLIN_CLASS" value="true" />
  </context>
</template>
```

### Example output
```kotlin
@Mock
lateinit var <name>: <type>
```

Where `<name>` and `<type>` are replaced on template insertion

## Run with mock annotation
```xml
<template name="runwithmock" value="@RunWith(MockitoJUnitRunner::class)" description="Run With Mock annotation" toReformat="false" toShortenFQNames="true">
  <context>
    <option name="KOTLIN_TOPLEVEL" value="true" />
  </context>
</template>
```

### Example
Adds `@RunWith(MockitoJUnitRunner::class)` to a class

## Test function
```xml
<template name="testfun" value="@Test&#10;fun test_$NAME$() {&#10;    // Given&#10;    $END$&#10;    &#10;    // When&#10;    &#10;    &#10;    // Then&#10;    &#10;}&#10;" description="Test function" toReformat="true" toShortenFQNames="true">
  <variable name="NAME" expression="" defaultValue="" alwaysStopAt="true" />
  <context>
    <option name="KOTLIN_CLASS" value="true" />
  </context>
</template>
```

### Example
```kotlin
@Test
fun test_<name>() {
  // Given
  <end>
  // When

  // Then
}
```

Where `<name>` is replaced on template insertion, and `<end>` is where the cursor will be placed once
the name has been set (press enter after entering the name).

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
