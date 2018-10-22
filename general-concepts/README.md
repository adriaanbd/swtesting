## Introduction

Finding failures in software is important. If we can find and fix them, we'll eventually run out of bugs and have software that works reliably.

Its intimidating to look at software testing from a macro perspective, as a large problem, if you consider that neither Microsoft or Google were able to eliminate all the bugs in their products. So, how could we possibly resolve all of the bugs in our software, considering neither of those two tech giants resolved theirs?

This is achieved by looking at it from a micro perspective, since the testing problem is not a large problem but rather a collection of small problems, wherein known techniques that have been and are being used in the industry could be applied.

## What is testing?
From a general perspective, a software under test requires a source of test input(s) to be able to produce test output(s), which is processed through an acceptability check.

If the outputs are okay, conforming to a previously defined acceptability criteria, the test case succeeds. Otherwise, the software under test has to be debugged.

Keep in mind that it's always a challenge to:

1. Select a good set of test inputs, and
2. Design a good acceptability test

## What happens when we test software?
The output produced by a test case determines the actions taken thereafter:

If the output is okay, we run another test case.
If the output isn't okay, we have to discover what happened.
We don't learn much from the first outcome, it just raises our confidence. However, we learn a lot more from the second outcome because we'll have to check if there is a bug in either of the following, in order:

1. The software under test (SUT).
2. Our acceptability test.
3. Our specification(s).
4. The Operating System, Compiler, Libraries, Hardware.

If we realize we're dealing with the fourth outcome above, we'll need to figure out a workaround. However, if the bug isn't found in either of the above-mentioned options, we have some sort of impasse.

We can conclude at this point by saying that a test is a tiny experiment wherein a failed test reveals that *something is flawed* in either *our understanding* of the system *or a part of the system*.

## A famous real-life example: The Mars Climate Orbiter

The Mars Climate Orbiter, a 745lb robotic space probe, was launched by NASA on December 11, 1998 to study the Martian climate, atmosphere and surface, and act as a communications relay. However, on September 23, 1999 communication with the spacecraft was lost as the spacecraft went into orbital insertion, due to ground-based computer software which produced output in non-SI units (U.S. standard) of pound-force seconds instead of SI units (International standard) of newton seconds, as specified in the contract between NASA and Lockheed Martin. Sadly, the spacecraft encountered Mars on a trajectory that brought it too close to the planet, causing it to pass through the upper atmosphere and disintegrate.

What happened was that NASA's software expected units in metric (meters per second), but Lockheed Martin's software was programmed in english units (feet per second). And although the underlying code was actually correct, the miscommunication caused the Mars Climate Orbiter to drift and crash into the Mars atmosphere.

So, where was the bug? Was it in the SUT, acceptability test, specification and/or OS, compiler, libraries and hardware?

## A practical example: the Fixed Sized Queue

Take a look at the code below:
```
import array

class Queue:
"""
Provides a fized-size FIFO (first-in-first-out) queue of integers
"""
    def __init__(self, size_max):
    """
    Takes a single parameter: an integer > 0 that is the maximum number of elements the queue can hold
    """
        assert size_max > 0  # make sure that int is greater than 0
        self.max = size_max  # save a temporary copy of it
        self.head = 0  # head pointer, indicates the element to dequeue (first in, first out)
        self.tail = 0  # tail pointer, indicates where the next element should be loaded
        self.size = 0  # number of objects currently in the queue
        self.data = array.array('i', range(size_max))

    def empty(self):
    """
    returns True if the queue holds no elements
    """
        return self.size == 0

    def full(self):
    """
    returns True if the queue cannot hold any more elements
    """
        return self.size == self.max

    def enqueue(self,x):
    """
    attempts to put the integer x into the queue; it returns True if successful and False if the queue is full
    """
        if self.size == self.max:
            return False
        self.data[self.tail] = x
        self.size += 1
        self.tail += 1
        if self.tail == self.max:
            self.tail = 0
        return True

    def dequeue(self):
    """
    removes an integer from the queue and returns it, or else returns None if the queue is empty
    """
        if self.size == 0:  #
            return None
        x = self.data[self.head]
        self.size -= 1  #
        self.head += 1
        if self.head == self.max:
            self.head = 0
        return x
```

This is a fixed size queue data structure that supports an enqueue operation and a dequeue in which enqueue elements get dequeued in a *first in first out* order. So, if we enqueue(7) and then enqueue(8), the first thing we can dequeue is 7, and then 8. If we try to dequeue an empty queue, we would get some type of error.

In case you're wondering: why not implement a python list that already support enqueue and dequeue operations? The problem with the python list is that they are dynamically allocated and dynamically typed, which doesn't make them very fast. The above-mentioned code implements a fixed size storage region of statically typed memory (the *i* argument of ```array.array('i', range(size_max))``` means that only *integers* are able to be stored), thereby avoiding some of Python's dynamic checks that make the queue slow.