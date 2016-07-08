---
layout: post
title:  "Using Kotlin's sealed class to approximate Swift's enum with associated data"
date:   2016-07-08 14:23:00 +0100
categories: kotlin
---

Swift has a great feature for an `enum` type that allows you to associated data
with it.

Here is the example given on the [Swift Programming Language](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html#//apple_ref/doc/uid/TP40014097-CH12-ID148) site:

```swift
enum Barcode {
    case UPCA(Int, Int, Int, Int)
    case QRCode(String)
}
```

This should be read as:
> Define an enumeration type called Barcode, which can take either a value of UPCA with an associated value of type (Int, Int, Int, Int), or a value of QRCode with an associated value of type String.

Here is an example of creating an instance of the _UPCA_ type:

```swift
var productBarcode = Barcode.UPCA(8, 85909, 51226, 3)
```

## Trying to emulate this in Kotlin

### Via interfaces

In looking for a way to implement this in Kotlin we initially used an `interface` as a type indicator and then created classes for each type value that defined it's required properties. Here is the `Barcode` example using `interface` in Kotlin:


```kotlin
interface Barcode
class UPCA(val numberSystem: Int, val manufacturer: Int, val product: Int, val check: Int) : Barcode
class QRCode(val productCode: String) : Barcode
```

This could then be used in a function such as:

```kotlin
fun barcodeAsString(barcode: Barcode): String =
	when (barcode) {
		is UPCA -> "${barcode.numberSystem} ${barcode.manufacturer} ${barcode.product} ${barcode.check}"
		is QRCode -> "${barcode.productCode}"
		else -> "Unknown"
	}
```

But since anything can implement the `Barcode` interface the compiler is unable to determine if the `when` statement handles all possible cases so the `else` is required.

### Using a `sealed` class

Now if we modify the implementation to use a [`sealed class`](https://kotlinlang.org/docs/reference/classes.html#sealed-classes) it is possible to improve our type safety and remove that extraneous `else`.

The example now becomes:

```kotlin
sealed class Barcode {
	class UPCA(val numberSystem: Int, val manufacturer: Int, val product: Int, val check: Int) : Barcode()
	class QRCode(val productCode: String) : Barcode()
}
```

and

```kotlin
fun barcodeAsString(barcode: Barcode): String =
	when (barcode) {
		is Barcode.UPCA -> "${barcode.numberSystem} ${barcode.manufacturer} ${barcode.product} ${barcode.check}"
		is Barcode.QRCode -> "${barcode.productCode}"
	}
```

The compiler is now able to check that all `Barcode` types have been catered for in the `when` statement so the `else` block is no longer required.