What’s new in Google’s V8 JavaScript engine Version 6.9

A new beta of Google’s V8 JavaScript engine is now available, for the Chrome 69 browser.

V8 is a staple in both the Chrome browser and the Node.js JavaScript runtime. WebAsembly is also supported in Mozilla Firefox, Apple Safari, and Microsoft Edge, though those browsers do not use V8.

[ Go deeper at InfoWorld: Beyond jQuery: An expert guide to JavaScript frameworks • The complete guide to Node.js frameworks • The 10 essential JavaScript developer tools • The 6 best JavaScript IDEs and 10 best JavaScript editors. | Keep up with hot topics in programming with InfoWorld’s App Dev Report newsletter. ]
Future version: New features in Google V8 Version 6.9 beta
The beta of V8 Version 6.9 arrived on August 7, with the production release due in a few weeks in coordination with the planned arrival of the Chrome 69 stable browser. Memory and performance improvements are the focus of the Version 6.9 beta Google’s JavaScript engine.

For memory savings, Version 6.9 offers embedded built-ins for x64-based computers. These are functions shared by all isolates and embedded onto the binary itself instead of being copied onto the JavaScript heap, thus existing in memory only once regardless of how many isolates are running. V8’s designers have seen an average 9 percent reduction of the heap size across the top 10,000 websites on x64 computers. Support for other platforms will follow in later releases.

For performance, V8 Version 6.9 reduces Mark-Compact garbage collection pause times by improving WeakMap processing. Concurrent and incremental marking now can process WeakMaps. Previously, this work was done in the final atomic pause of Mark-Compact garbage collection. The garbage collection now also does more work in parallel to lower pause times.

For performance, DataView methods have been reimplemented in V8 Torque, sparing a costly call to C++ compared to the previous runtime implementation. Also, calls to DataView methods now are inlined when compiling JavaScript into the TurboFan optimizing compiler. This provides better peak performance for hot code.

V8 Version 6.9 also includes Liftoff, a baseline compiler for the WebAssembly portable code format. It is enabled by default and intended to reduce startup times for WebAssembly-based apps by generating code as quickly as possible. Code itself quality is a secondary priority for Liftoff, with code eventually to be recompiled by V8’s TurboFan compiler.

[ JavaScript is the most widely deployed language in the world. Whether you're a beginning, intermediate, or advanced JavaScript developer, you'll master new skills with this nine-part course from PluralSight. ]
Liftoff was developed to address an issue in which the back end of the compilation process for TurboFan consumed a lot of time and memory, reducing performance of the WebAssembly code. Liftoff avoids the time and memory overhead of intermediate representation, generating machine code in a single pass over the bytecode of a WebAssembly function. Liftoff and Turbofan give V8 two compilation tiers, with Liftoff a baseline compiler for fast startup and TurboFan providing optimization for performance.

Google also plans to further improve startup time, cut memory consumption, and bring benefits of Liftoff to more users. These plans involve ports to ARM processors, for use on mobile devices. Liftoff currently works only on Intel 32- and 64-bit platforms. Other improvements under consideration include:

Implementing dynamic tier-up for mobile devices, to accommodate lower memory volumes on these devices. Experiments are proceeding with a combination of lazy compilation with Liftoff and dynamic tier-up of hot functions in TurboFan.
Improving Liftoff code generation performance and improving the  generated code as well.
Where to download the V8 6.9 beta
You can download the V8 6.9 beta from the Google Git repo.

Current version: What’s new in V8 Version 6.8
Google V8 Version 6.8, released in July 2018, focuses on performance and memory usage.

Performance has been boosted by array destructuring improvements. The optimizing compiler had not been generating ideal code for array destructuring, so the builders of V8 blocked escape analysis to eliminate temporary allocation, which made array destructuring with a temporary array as fast as a sequence of assignments.

A new implementation of Object.assign improves performance, via implementation of a fast path for JavaScript.

Performance for TypedArrays has been increased in instances when sorting is done using a comparison function.

Other new features in V8 Version 6.8 include:

To improve execution speed with the WebAssembly portable code format, developers can use trap-based bounds checking, a memory-management optimization, on Linux x64 platforms.
Memory consumption of SFIs (SharedFunctionInfo) has been reduced, via compression and removal of unnecessary fields.
Also to improve memory capabilities, a dependency on SFIs has been broken in which SFIs were unnecessarily kept alive, which had led to the risk of memory leaks.
Where to download the Google V8 6.8
You can download the V8 6.8 code from the Google Git repo. 

Previous version: What’s new in V8 Version 6.7
Google’s V8 JavaScriptengine is getting enhancements for language features and security with the Version 6.7 branch, which is now in production release.

The V8 6.7 engine has BigInt support enabled by default. Expected in a future version of ECMAScript, BigInts serve as a numeric primitive in JavaScript to represent integers with arbitrary precision. With BigInt, it is possible to perform integer arithmetic without overflowing. BigInt could serve as the basis of an eventual BigDecimal implementation, useful for representing sums of money with decimal precision.

Also featured in V8 6.7 are more mitigations for side-channel vulnerabilities, intended to prevent information leaks to untrusted code for JavaScript and WebAssembly.

Where to download the Google V8 6.7
You can download V8 6.7 from the Google Git repo.

Previous version: What’s new in V8 Version 6.6
Version 6.6 of Google’s V8 JavaScript engine focuses on JavaScript language features and code-caching capabilities.

For JavaScript, Function.prototype.toString() returns exact slices of source code text, including whitespace and comments. V8 Version 6.6 also implements String.prototype.trimStart() and String.prototype.trimEnd(). This capability had been available through nonstandard trimLeft() and trimRight() methods, which remain as aliases of the new methods to enable backward compatibility.

Additionally, line and paragraph separator symbols can be used in string literals, thus matching JSON. Previously, these had been treated as line terminators in string literals and their usage resulted in a SyntaxError exception.

The Array.prototype.values method gives arrays the same iteration interface as the ECMAScript 2015 Map and Set collections. These can be interacted over by keys, values, or entries by calling the same-named method. This change could be incompatible with existing JavaScript code; developers who find odd or broken behavior on a website can try to disable this feature via chrome://flags/#enable-array-prototype-values.

In another JavaScript programming improvement, the catch clause of try statements can be used without a parameter, which is useful if there is no need for an exception object in the code handling the exception.

Aside from JavaScript, a code cache after execution capability in Version 6.6 lets more functions be included in the cache, with functions no longer needing to be compiled on future page loads. Compile and parse times in hot load scenarios—in which Chrome visits a page for a third time and provides code previous cached—are reduced. As a result, loading is faster and smoother.

Other capabilities featured in V8 Version 6.6 include:

More mitigations to prevent information leaks to untrusted JavaScript and WebAssembly code.
Compile times have been improved by moving out or deprecation of remaining functionality related to AST numbering. A previous compilation process had required a postparsing stage dubbed AST numbering, where nodes in a syntax tree were numbered so compilers using it would have a common point of reference. But this postprocessing pass had ballooned to include other functionality. A new pipeline introduced last year eliminated the need for the numbering, but the numbering pass had remained until Version 6.6.
Asynchronous and array performance has been improved.
Where to download Google V8 Version 6.7
You can download V8 Version 6.7 from Google’s Git repo.

Previous version: What’s new in V8 Version 6.5
Released in February 2018, in V8 Version 6.5’s streaming compliation, WebAssembly modules are compiled while module bytes are still being downloaded. When all bytes of a single function have been downloaded, the function is passed to a background thread for compilation. As a result, WebAssembly compilation in Chrome 65 can maintain a 50Mbps download speed on high-end machines, Google says—meaning that if WebAssembly is downloaded at that speed, compilation finishes as soon as the download is done.

 Other improvements planned for the 6.5 branch include:

An untrusted code mode, developed in response to a specualative side-channel Spectre attack. This mode is suitable for applications processing user-generated, untrusted code and is enabled by default.
A mechanism to detect and prevent a deoptimization loop. This loop occurs when optimized code deoptimizes and there is no way to find out what went wrong. V8 developers also have inlined many JavaScript builtins that had been excluded because of a side effect between the load of a function to call and the call itself.
Where to download Google V8 Version 6.5
You can download the V8 Version 6.5 branch from Google’s Git repo.


注：转载https://www.infoworld.com/article/3252818/web-development/whats-new-in-googles-v8-javascript-engine.html?upd=1538873618909

