Sequencer.swift
===============

Sequencer is an iOS library for asynchronous flow control written in Swift.

Sequencer turns complicated nested blocks logic into a clean, straightforward, and readable code.

This is a direct port of the [Obj-C Sequencer](https://github.com/berzniz/Sequencer).

## Jumping straight to code

Let's output some strings to the Log Console:

```objc
let sequencer = Sequencer()
sequencer.enqueueStep() { result, next in
    println("* 1st Step")
    next(24)
}
sequencer.enqueueStep() { result, next in
    println("* 2nd Step")
    println("Got \(result) from previous step, let's double it")
    next(result as Int * 2)
}
sequencer.enqueueStep() { result, next in
    println("* 3nd Step - Async example")
    println("Got \(result) from previous step")
    println("This step is going to do some async work...")
    let popTime: dispatch_time_t = dispatch_time(DISPATCH_TIME_NOW, 2 * 1000);
    dispatch_after(popTime, dispatch_get_main_queue(), {
        println("finished the async work.");
        next(result);
    });
}
sequencer.enqueueStep() {
    println("* 4th Step - A short one")
    $1($0)
}
sequencer.enqueueStep() { result, next in
    println("* Final Step")
    println("Got \(result) from previous step")
}
sequencer.run()
```

What does the above code do?

1. A ```Sequencer``` was created.
2. Five steps were enqueued to the Sequencer. The third step is async, but all the rest are plain sync code.
3. Each step finishes by calling ```next()``` with a ```result``` object. This result is sent to the next step.
4. We ```run``` the sequencer.

__Note__: Break the steps by just removing the call to ```next(nil)```. Everything will be cleaned-up auto-magically.

## License

Sequencer is available under the MIT license:

Copyright (c) 2013 Tal Bereznitskey

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

## Contact

Find me on github: [Tal Bereznitskey](http://github.com/berzniz)

Follow me on Twitter: [@ketacode](https://twitter.com/ketacode)