# Software Testing
This space is dedicated to Software Testing, a place to keep notes and scripts for further reference.

## Introduction
Finding failures in software is important. If we can find and fix them, we'll eventually run out of bugs and have software that works reliably.

Its intimidating to look at software testing from a macro perspective, as a *large* problem, if you consider that neither Microsoft or Google were able to eliminate all the bugs in their products. So, how could we possibly resolve all of the bugs in our software? 

This is achieved by looking at it from a micro perspective, since the testing problem is not a *large* problem but rather a collection of *small* problems, wherein known techniques that have been and are being used in the industry could be applied. 

## What is testing?

From a general perspective, a software under test  requires a source of test input(s) to be able to produce test output(s), which is processed through an acceptability check. 

If the outputs are okay,  conforming to a previously defined acceptability criteria, the test case succeeds. Otherwise, the software under test has to be debugged.

Keep in mind that it's always a challenge to:
 - select a good set of test inputs, and
 - design a good acceptability test

## What happens when we test software?

The output produced by a test case determines the actions taken thereafter:

 1. If the output is okay, we run another test case.
 2. If the output isn't okay, we have to discover what happened.

We don't learn much from the first outcome, it just raises our confidence. However, we learn a lot more from the second outcome because we'll have to check if there is a bug in either of the following, in order:

 1. The software under test.
 2. Our acceptability test.
 3. Our specification(s).
 4. The Operating System, Compiler, Libraries, Hardware, etc.

If we realize we're dealing with the fourth outcome above, we'll need to figure out a workaround. However, if the bug isn't found in either of the above-mentioned options, we have some sort of impasse.

We can conclude at this point by saying that a test is a tiny experiment wherein a failed test reveals that something is *flawed* in either *our understanding* of the system or *a part* of the system.  
