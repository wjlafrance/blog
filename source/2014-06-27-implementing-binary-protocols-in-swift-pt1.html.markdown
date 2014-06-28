---
title: Implementing Binary Protocols in Swift (Part 1)
---

Many people in our community have played great titles such as [Diablo II](http://us.blizzard.com/en-us/games/d2/) and [WarCraft III](http://us.blizzard.com/en-us/games/war3/). During the mid-2000s, there was a small community of young developers building third-party chat clients for the [Battle.net](https://en.wikipedia.org/wiki/Battle.net) gaming and matchmaking service. The most famous of these clients, [StealthBot](http://www.stealthbot.net/wiki/StealthBot), was developed right here in Madison, WI.

READMORE

These games shared a protocol for communicating with the Battle.net chat servers (frequently called "BNCS"), which was reverse engineered and thoroughly documented at a website called BnetDocs. The original site has long since disappeared from the internet, but a mirror with updated content can be found [here](https://bnetdocs.org/). There are several data types used in the protocol, described [here](https://bnetdocs.org/?op=doc&did=19), that we'd like to implement in Swift.

The protocol mostly consists of writing integers from memory straight to a socket. Heres how I [implemented this in Objective-C](https://raw.githubusercontent.com/wjlafrance/bnetclient/01bf4abb2171e74aa1326edb73778ac7e3b0049c/BnetClient/NSMutableData+PacketBuffer.m), as a category on `NSMutableData`:

    #define PUSH_INT [self appendBytes:&value length:sizeof(value)];
    - (void)writeUInt8:(uint8_t)value { PUSH_INT }
    - (void)writeUInt16:(uint16_t)value { PUSH_INT }
    - (void)writeUInt32:(uint32_t)value { PUSH_INT }
    - (void)writeUInt64:(uint64_t)value { PUSH_INT }

To expand that macro for readability:

    - (void)writeUInt8:(uint8_t)value {
        [self appendBytes:&value length:sizeof(uint8_t)];
    }
    - (void)writeUInt16:(uint16_t)value {
        [self appendBytes:&value length:sizeof(uint16_t)];
    }
    ...

This technique, while consise, treats integers as `void*` in an unsafe way. How can we implement this in Swift? Rather than build on top of `NSMutableData`, I decided to define my buffer as a concrete class, wrapping an `Array<UInt8>`.

    class Buffer : Printable {
        var contents: Array<UInt8> = []

        func add(value: UInt8) {
            contents.append(value)
        }

Adding a `UInt8` into the array works nicely, but with larger integers we need to be deliberate about what order the bytes are added. Additionally, we need to ensure that we don't overflow, which would cause a runtime exception in Swift rather than simple truncation in C. To keep methods short, I split each integer into it's upper and lower half and call the method for the next smaller size.

        func add(value: UInt16) {
            add(UInt8(value >> 8 & 0xFF))
            add(UInt8(value >> 0 & 0xFF))
        }
        
        func add(value: UInt32) {
            add(UInt16(value >> 16 & 0xFFFF))
            add(UInt16(value >>  0 & 0xFFFF))
        }
        
        func add(value: UInt64) {
            add(UInt32(value >> 32 & 0xFFFFFFFF))
            add(UInt32(value >>  0 & 0xFFFFFFFF))
        }

Reading integers back from the buffer is essentially the opposite operation.

        func readUInt8() -> UInt8 {
            return contents.removeAtIndex(0)
        }
        
        func readUInt16() -> UInt16 {
            return UInt16(readUInt8()) << 8 | UInt16(readUInt8())
        }
        
        func readUInt32() -> UInt32 {
            return UInt32(readUInt16()) << 16 | UInt32(readUInt16())
        }
        
        func readUInt64() -> UInt64 {
            return UInt64(readUInt32()) << 32 | UInt64(readUInt32())
        }

This implementation is still incomplete. Next week, I should have a working example of connecting to Battle.net and emulating the original Diablo client. Stay tuned!
