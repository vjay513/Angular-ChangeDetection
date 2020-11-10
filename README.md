
# Angular Chnage Detection Mechanism
 
 ## What is Angular change detection mechanism and how transparent it is ?

whenever user click or do any action in the window, we could able to see the reflection in the browser, we call that reflection as re-render or data binding.
 what factor helps browser,

if we want, we can override javascript run time execution, this nature helps us to achieve the change detection in Angular.

this mechanism replace low level functionalities like addEventListner,

```
function addEventListener(eventName, callback){
  callRealAddEventListener(eventName, function() {
    callback(...);
    var changed = angular.runChangeDetection();
    if(changed){
      angular.renderUIPart();
    }
  });
}
```

within this pesudo code, what we can understand, this angular guy letting allow javascript nature and indication browser changes as well.

okay, so pathing working happening behind the angular,

## How really this patching work will work?

there is helper library, his name is zone.js is friend of angular,

what this guy really do, **A zone is nothing more than execution context that survives multiple javascript VM execution turns**

what is virtual machine(VM), In chrome **Java Script** engine devtools uses the text **VM** concatenated with the script Id as a title for these scripts. so instances of the VM.script class contain precompiled scripts that can be executed in specific contexts.

zone.js also maintaining the profilling or keepimg track of long stack traces that run across multiple VM turns.

## Browser Async API support

- All bowser events, setTimeout, setInterval and Ajax requests
- Even websockets also patched by zone.js
- IndexDb callbacks are not part of change detection

## The change Detection tree

when we walk through the stack trace then we can see the change detection in action.

## Why does change detection work like this by default?

If you are familiar with Angular 1, think about $digest() and $apply() and all the pitfalls of when to use them / not to use them. One of the main goals of Angular is to avoid that

## What about comparison by reference?

The fact of the matter is that Javascript objects are mutable, and Angular wants to give full support out of the box for those.

This all has to do with the way the Javascript virtual machine works. The code for dynamically comparing properties, although generic cannot easily be optimized away into native code by the VM just-in-time compiler.

This is unlike the specific code of the change detector, which does explicitly access each of the component input properties. This code is very much like the code we would write ourselves by hand, and is very easy to be transformed into native code by the virtual machine.

The end result of using generated but explicit detectors is a change detection mechanism that is very fast (more so than Angular 1), predictable and simple to reason about.

## The OnPush change detection mode


```@Component({
    selector: 'todo-list',
    changeDetection: ChangeDetectionStrategy.OnPush,
    template: ...
})
export class TodoList {
    ...
} 
```

When using OnPush detectors, then the framework will check an OnPush component when any of its input properties changes, when it fires an event, or when an Observable fires an event


Using Immutable.js to simplify the building of Angular apps

## How to trigger a change detection loop in Angular?

```
ngAfterViewChecked() {
    if (this.callback && this.clicked) {
        console.log("changing status ...");
        this.callback(Math.random());
    }
}
```

We really have to go out of our way to trigger a change detection loop, but just in case its better to always use development mode during the development phase, as that will avoid the problem.

This guarantee comes at the expense of Angular always running change detection twice, the second time for detecting this type of cases. In production mode change detection is only run once.

## turning on/off change detection, and triggering it manually


There could be special occasions where we do want to turn off change detection. Imagine a situation where a lot of data arrives from the backend via a websocket. We might want to update a certain part of the UI only once every 5 seconds. To do so, we start by injecting the change detector into the component:

```
constructor(private ref: ChangeDetectorRef) {
    ref.detach();
    setInterval(() => {
      this.ref.detectChanges();
    }, 5000);
  }
 ```
 
 the mechanism of detecting changes in a component is much faster due to the way change detectors are built.
 
 Finally and unlike in Angular 1, the change detection mechanism is customizable.



