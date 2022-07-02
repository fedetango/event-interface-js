# event-interface-js
A simple event interface for Javascript.  Applies events socket to your objects, classes, and environments.

The event-interface is a single script without third party libraries.
Hopefully in the future ECMA implements it natively.

## Quick use

```Javascript

let e = new EventInterface();

// Define async event
e.on('custom-event', function(arg1, arg2){
  // Fire a callback function every time an event fires
});

// Define sync event
let r = await e.when('custom-event'); // Wait to next fire event

// Fire event
e.launch('custom-event', arg1, arg2); // Fire a trigger event

// Remove listener
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

// Note that the launch function is not inherited by the object. If you want to build a link for the user, you will need to create a reference manually.

```

## Apply Event Interface in a class

```Javascript
import { EventInterface } from 'event-interface';


// Implement in a class
class Teapot() {
  
  // We suggest saving the EventInterface instance in a private property
  #events;
  constructor() {
    this.#events = EventInterface.implement(this);
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

## Event inheritance
You will often need extended classes to access and execute events, which can create collisions.

EventInterface takes this into account and addresses it with a number of functions.

```Javascript
 
```

## Private Events
Too is posible declarate and trigger private events.

```Javascript

```
