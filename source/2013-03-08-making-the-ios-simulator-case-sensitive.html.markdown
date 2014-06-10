---
title: Making the iOS Simulator case sensitive
---

Hello from [CocoaConf](http://cocoaconf.com)!

In Dave Koziol’s iOS Debugging session, he pointed out that the iOS Simulator does not have a case sensitive file system. As the actual device does have a case sensitive file system, this allows bugs to enter your application unnoticed.

Here’s a few scripts to create and mount a RAM disk in place of the iOS Simulator directory. Please close the simulator completely before running these scripts, and note that this will reset your simulator. You could persist the data if you wanted, but I notice that the simulator gets wonky after too long anyhow.

Mounting Simulator on RAM-disk:

    pushd ~/Library/Application\ Support
    diskutil erasevolume hfsx "Simulator" `hdiutil attach -nomount ram://2314836`
    rm -rf iPhone\ Simulator
    ln -s /Volumes/Simulator/ iPhone\ Simulator
    popd

Returning to normal:

    pushd ~/Library/Application\ Support
    rm -rf iPhone\ Simulator
    hdiutil detach /Volumes/Simulator
    popd

Don’t take my word for it:

    int main(int argc, char *argv[])
    {
        @autoreleasepool {
            NSFileManager *fileMan = [NSFileManager defaultManager];
            NSURL *docs = [fileMan URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask][0];

            NSURL *lowercaseDocUrl = [docs URLByAppendingPathComponent:@"lowercase.txt"];
            NSURL *uppercaseDocUrl = [docs URLByAppendingPathComponent:@"LOWERCASE.TXT"];

            assert(![fileMan fileExistsAtPath:lowercaseDocUrl.path]);
            assert(![fileMan fileExistsAtPath:uppercaseDocUrl.path]);
            assert([@"Hello" writeToURL:lowercaseDocUrl atomically:YES encoding:NSASCIIStringEncoding error:nil]);
            assert([fileMan fileExistsAtPath:lowercaseDocUrl.path]);
            assert(![fileMan fileExistsAtPath:uppercaseDocUrl.path]); // will crash on case-insensitive filesystem

            NSLog(@"All assertions passed. We're case sensitive.");

            return 0;
        }
    }
