## ERROR: $rootScope:inprog
## Action Already In Progress

### Description

> At any point in time there can be only one $digest or $apply operation in progress. This is to prevent very hard to detect bugs from entering your application. The stack trace of this error allows you to trace the origin of the currently executing $apply or $digest call, which caused the error.

### Digest Phases

Angular keeps track of what phase of processing we are in, the relevant ones being $apply and $digest. Trying to reenter a $digest or $apply while one of them is already in progress is typically a sign of programming error that needs to be fixed. So Angular will throw this error when that occurs.

In most situations it should be well defined whether a piece of code will be run inside an $apply, in which case you should not be calling $apply or $digest, or it will be run outside, in which case you should wrap any code that will be interacting with Angular scope or services, in a call to $apply.

As an example, all Controller code should expect to be run within Angular, so it should have no need to call $apply or $digest. Conversely, code that is being trigger directly as a call back to some external event, from the DOM or 3rd party library, should expect that it is never called from within Angular, and so any Angular application code that it calls should first be wrapped in a call to $apply.


[Angular Doc](https://docs.angularjs.org/error/$rootScope/inprog?p0=NaNigest)

### Common Causes

1. Inconsistent API (Sync/Async)
2. Triggering Events Programmatically

> It is possible to workaround this problem by using $timeout(fn, 0, false).


