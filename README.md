# event-interface-js
A simple event interface for Javascript.  Applies events socket to your objects, classes, and environments.

The event-interface is a single script without third party libraries.
Hopefully in the future ECMA implements it natively.

## Quick use

```Javascript

let e = new EventInterface();

e.on('custom-event', function(arg1, arg2){
  // Fire a callback function every time an event fires
});

let r = await e.when('custom-event'); // Wait to next fire event

e.launch('custom-event', arg1, arg2); // Fire a trigger event

e.removeListener(callbackID); // Remove a record callback

```

## Apply interface in a object

The "implement" static function provided by EventInterface class allows to make compatible any object with the event interface automatically. 

```Javascript

// (return a EventInterface object instance)
let ei = EventInterface.implement(obj);

// Now you can call following functions on the object:
//obj.on();
//obj.when();
//obj.removeCallback();

// To execute launches use the created instance of EventInterface
//ei.launch();

```

## Apply Event Interface in a class

```Javascript
import { EventInterface } from 'event-interface';


// Implement in a class
class Teapot() {
  
  // We suggest saving event in a private property
  #events;
  constructor() {
    #events = EventInterface.implement(this);
  }
  
  // To warm the tea and then it cools.
  toWarm(temp=20) {
    setTimeout(function(){
      this.#events.launch('ready', temp);
      this.#cool(temp);
    }.bind(this), temp * 100);
  }
  
  #cool(temp) {
    setTimeout(function(){
      this.#events.launch('cold', temp);   
    }.bind(this), temp * 50)
  }
  
}

let t = new Teapot();

t.on('ready', function(temp){
  console.log('The tea is ready.', 'Temp:', temp);
});

t.toWarn(30);
await t.when('cold');
console.log('The tea is cold. Heat again');

t.toWarn(50);
await t.when('cold');
console.log('The tea is cold. Finish');

```
