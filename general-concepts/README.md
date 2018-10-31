# Introduction

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

*In case you're wondering:* why not implement a python list that already supports enqueue and dequeue operations?  And this is due to the fact that Python lists are dynamically allocated and dynamically typed, which doesn't make them very fast. The above-mentioned code implements a fixed size storage region of statically typed memory (the *i* argument of ```array.array('i', range(size_max))``` means that only *integers* are able to be stored), thereby avoiding some of Python's dynamic checks that make the queue slow.

#### Test your understanding of the code:

Consider the following.

```
q = Queue(2)
r1 = enqueue(6)
r2 = enqueue(7)
r3 = enqueue(8)
r4 = dequeue()
r5 = dequeue()
r6 = dequeue()
```

What are the values of r1, r2, r3, r4, r5 and r6?

+ 6, 7, 8, 6, 7, 8
+ True, True, True, 6, 7, None
+ True, True, False, 6, 7, 8
+ True, True, False, 6, 7, None

After completing the above, take a moment to consider how useful is any individual test case and if there is a way to make a test case that is more useful in practice. 

#### Considering usefulness of a test case

Take a look at this code snippet:

```
enqueue(7)
x = dequeue()
if x == 7:
    print 'Succes!'
else:
    print 'Error!'
```

If we pass this test case, what have we learned about the code?

+ The code passes this test case
+ The code passes any test case where we replace 7 with a different integer
+ The code passes many test cases where we replace 7 with a different integer

Certainly, the code passes this test case and it passes many test cases where we replace 7 with a different integer, but it doesn't pass *any* test case where we replace 7 with a different integer. Say, for example, that we replace 7 with a gigantic integer that requires a lot of storage space, we might not get the same behavior out of this test case, thus we can't make that statement.

#### Equivalent Tests

Given the above-mentioned test case, is it possible to conclude that another test case or group of test cases will succeed?

For example, will the following code succeed:

```
# Example A:

enqueue(7)
sleep(100)  # make program stop for an x amount of time
x = dequeue()
if x == 7:
    print 'SUCCESS'

# Example B:

enqueue(any integer i)
x = dequeue()
ix x == i:
    print 'SUCCESS'
```

We can argue that since the Fixed Size Queue code did not make any dependencies on time or on the size of the integer, the above code snippet will succeed. 

Note that *equivalent tests* consist in making an argument about the form of the code, in which a single test case is *representative* of a whole class of actual executions of the system that we are testing. 

Thus the idea is that a single test case *represents* a point in the input space for the system that we are testing, and by running the code, we try to map that point in the input space to a point in the output space; however, consider that there is an extremely large input space of possibilities, and we certainly can't test it all, specially considering project constraints such as time. 

So, what we try to do is make an argument that if the system executes correctly for the single input that we have tried, it will execute correctly for all of the inputs in a class of inputs that are *closely related* or somehow *equivalent* for that particular system under test.

But before going there, let's consider if our conclusion of the above-mentioned snippet is incorrect:

Is it ever the case that our Queue could work for *some* integer but not or *all* integers?

Example A - If the Python run time contains a garbage collection bug that corrupted memory, it might be the case that ```sleep()``` might give our Python interpreter time to corrupt our value and the ```dequeue()``` operation will not get something out. 

Example B - Similarly, if the Python run time contains some bug 
where large integers are truncated, we won't necesarily get the same integer back in our ```dequeue()``` operation.

In both of these cases, we are making arguments about the broader runtime system and not the actual code under test. In summary, we can make these type of arguments, but we have to be really careful about the *assumptions* in which they are based on.

#### Testing the Queue

Consider the following test cases under and understand how they are testing the
Queue in some say.

```
def test1():
    q = Queue(3)
    res = q.empty()
    if not res:
        print "test1 NOT OK"
        return
    res = q.enqueue(10)
    if not res:
        print "test1 NOT OK"
        return
    res = q.enqueue(11)
    if not res:
        print "test1 NOT OK"
        return
    x = q.dequeue()
    if x != 10:
        print "test1 NOT OK"
        return
    x = q.dequeue()
    if x != 11:
        print "test1 NOT OK"
        return
    res = q.empty()
    if not res:
        print "test1 NOT OK"
        return
    print "test1 OK"

def test2():
    q = Queue(2)
    res = q.empty()
    if not res:
        print "test2 NOT OK"
        return
    res = q.enqueue(1)
    if not res:
        print "test2 NOT OK"
        return
    res = q.enqueue(2)
    if not res:
        print "test2 NOT OK"
        return
    res = q.enqueue(3)
    if q.tail != 0:
        print "test2 NOT OK"
        return
    print 'test2 OK'

def test3():
    q = Queue(1)
    res = q.empty()
    if not res:
        print "test3 NOT OK"
        return
    res = q.dequeue(10)
    if not res is None:
        print "test3 NOT OK"
        return
    res = q.enqueue(1)
    if not res:
        print "test3 NOT OK"
        return
    x = q.dequeue()
    if x != 1 or q.head != 0:
        print "test3 NOT OK"
        return
    print "test3 OK"
```

## Creating Testable Software

Some generic recommentations:

+ Write clean code
+ Refactor code
+ Always be able to describe what a module does and how it interacts with other code
+ No extra threads
+ No swamp of global variables
+ No pointer soup
+ Modules should have unit tests
+ When applicable, support fault injection
+ Assertions, assertions, assertions

### Assertions

An *assertion* is an executable check for a property that *must* be true.

An example:

```
def sqrt(arg):
    # compute result
    assert result >= 0
    return result
```

After the function computes some result, it first asserts that the result is greater than or equal to zero, and then returns the result. Why use this assertion? Because we know that the square root of some number should be a positive integer or equal to zero.

#### Assertion Guidelines:

1. Assertions are not for error handling - assertions are to assert that the
   result of the logic utilized is correct. It isn't about asserting something
   about the behavior of some entity of the program.

2. Never make assertions that have side effects - when we turn on optimization
   in Python, it'll drop all assertions in a program, thus making assertions
   useless because the program won't work as expected.

3. No silly assertions - for example, it wouldn't make any sense to ```assert
   (1+1) == 2``` because is it conceivable that in some python program 1+1 does
   not equal to 2? Sure, if the Python interpreter is broken, but if that's the
   case, nothing would work at all.

4. The best assertions are those which check a non-trivial property that could
   be wrong but only if we made a mistake in our logic. It isn't something
   wrong if the user did something wrong or something that's just silly to
   check.

#### Testing the Queue with Assertions