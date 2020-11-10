
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




