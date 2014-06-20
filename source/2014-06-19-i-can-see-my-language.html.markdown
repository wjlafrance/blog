---
title: I can see my language from here!
---

When Apple announced [Swift](https://en.wikipedia.org/wiki/Swift_(programming_language)) one of the first things I heard was "hey, they copied *feature* from *language*!" Wikipedia lists Objective-C, Rust, Haskell, Ruby, Python, C#, and CLU as the language's influencers, so it's not surprising that everyone sees something from their favorite language.

Let's take a look at various code samples in different languages and see how they compare to Swift.

---

It would be a violation of software tradition to not start with hello world.

Scala:

    println("Hello, world!")

Swift:

    println("Hello, world!")


Well, that's identical. How about constant and variable declarations and functions?

Scala:

    val explicitlyTypedConst: String = "you can't modify this!"

    def createGreeting(name: String): String = {
        return s"hello $name"
    }

    val formatArg = "world"
    var formattedString = createGreeting(formatArg)
    println(formattedString)

Swift:

    let explicitlyTypedConst: String = "you can't modify this!"

    func createGreeting(name: String) -> String {
        return "hello \(name)"
    }

    let formatArg = "world"
    var formattedString = createGreeting(formatArg)
    println(formattedString)

There's a lot of similarities here. We see the syntax of declarations is the exact same, taking the declaration keyword (`let` or `val` for a constant, and `var` for a variable) then the name, followed optionally by a colon and the type, then an assignment.

The function syntax is slightly different. In Scala, you specify the function's return type using `: Type` as you do with variables, whereas in Swift you specify return types using `-> Type`. The presence of the equals sign in Scala is another interesting difference.

---

One of my favorite features of the language is the syntax for properties. I'm a big fan of this syntax because it keeps your setters and getters clearly connected to eachother. I don't have a C# compiler handy, so let's borrow [an example from MSDN](http://msdn.microsoft.com/en-us/library/x9fsa0sw.aspx).

C#:

    class TimePeriod {
        private double seconds;

        public double Hours {
            get {
                return seconds / 3600;
            }
            set {
                seconds = value * 3600;
            }
        }
    }

Swift:

    class TimePeriod {
        var seconds: Int = 0
        
        var hours: Int {
            get {
                return seconds / 3600
            }
            set(newHours) {
                seconds = newHours * 3600
            }
        }
    }

---

But constructing an entire TimePeriod class for this purpose seems a little Javaish. Those coming from Rails might be expecting to type `5.minutes`, a convenience added onto Ruby's `FixNum`. Let's [monkey patch](https://en.wikipedia.org/wiki/Monkey_patch) Swift's `Int` by adding read-only properties.

Swift:

    extension Int {
        var milliseconds: Int { return self; }
        var seconds: Int { return self.milliseconds * 1000 }
        var minutes: Int { return self.seconds * 60 }
        var hours: Int { return self.minutes * 60 }
    }

    var timeInterval = 3.hours + 2.minutes + 15.seconds

---

One of the most common tasks I find myself doing is iterating over a collection. Let's say hello to everyone.

Java:

    List<String> people = ImmutableList.of("Alice", "Bob", "Charlie", "Deb");
    for (String person : people) {
        System.out.format("Hello %s\n", person);
    }

Objective-C:

    NSArray *people = @["Alice", "Bob", "Charlie", "Deb"];
    for (NSString *person in people) {
        NSLog(@"Hello, %@", person);
    }

Swift:

    var people = ["Alice", "Bob", "Charlie", "Deb"]
    for person in people {
        println("Hello \(person)")
    }

Here we see the same basic syntax, using the `for` keyword, indicating the variable name we'd like each element to be exposed to our loop body as, the collection we're iterating, and then the action to take for each element.

---

Tuples are a feature that I haven't seen commonly. Here's a classic example of swapping two integers using a temporary storage.

C:

    #include <stdio.h>

    void swap(int *a, int *b) {
        int tmp = *a;
        *a = *b;
        *b = tmp;
    }

    int main(int argc, char **argv) {
        int a = 15;
        int b = 30;
        swap(&a, &b);
        printf("a = %d, b = %d\n", a, b);
    }

In this code we're passing two integers by-reference into another function. The `swap` function is not generic and can only swap `int`s as well. We can take advantage of Ruby's tuples to swap variables very easily.

Ruby:

    a = 15
    b = 30
    b, a = a, b
    puts "a = #{a}, b = #{b}"

Swift incorporates this syntax as well.

    var a = 15
    var b = 30
    (a, b) = (a, b)
    println("a = \(a), b = \(b)")

---

I hope you've enjoyed this very brief tour of some language features in Swift. If you look at Swift and see your favorite language in there somewhere, you're not alone.

