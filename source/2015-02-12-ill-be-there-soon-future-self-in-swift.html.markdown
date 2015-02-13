---
title: I'll be there soon - a future self in Swift
---

Working with Cocoa APIs in Swift is a mostly pleasant experience, even more so with the [upcoming Objective-C changes](https://developer.apple.com/swift/blog/?id=22) to express nullability.

There's one case that really irks me though:

    import CoreBluetooth

    class MyBluetoothClass: CBCentralManagerDelegate {

        let centralManager: CBCentralManager

        init() {
            centralManager = CBCentralManager(delegate: nil, queue: dispatch_get_main_queue())
            super.init()
            centralManager.delegate = self
        }
    }

In this case, `CBCentralManager`'s constructor takes a delegate. In Objective-C I usually unsafely pass `self`. This is unsafe because `-initWithDelegate:queue:)` has the ability to send messages to us before we finish our initialization. In practice, we know that it doesn't, so we just ignore the safety rules and do it anyhow.

Swift won't let you allow that.

A possible solution would be to introduce the concept of a future or promised self. It would behave like an ARC weak reference, being `nil` when unable to be messaged, with an additional feature: until initialization finishes the passed object can not be resolved.

I'm not sure exactly how this would work into the language's syntax, but consider how it would impact the above example:

    import CoreBluetooth

    class MyBluetoothClass: CBCentralManagerDelegate {

        let centralManager = CBCentralManager(delegate: futureSelf, queue: dispatch_get_main_queue())

    }

Please feel free to discuss this idea on Twitter, where you can find me [@wjlafrance](http://twitter.com/wjlafrance). I'd like to hear any possible implementation details, or flaws with the concept.
