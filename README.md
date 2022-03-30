# event-interface-js
A simple event interface for Javascript.  Applies events socket to your objects, classes, and environments.

The event-interface is a single script without third party libraries.
Hopefully in the future ECMA implements it natively.

## QuickUse

```Javascript

// Create custom event interface
let e = new EventInterface();

// Fire a callback function every time an event fires
e.on('custom-event', function(arg1, arg2){
  
});

// Wait to next fire event
let p = await e.when('custom-event');

// Fire event
e.launch('custom-event', arg1, arg2);

// Remove a callback
e.removeCallback(callbackID);

// Apply event interface in a object
EventInterface.implement(obj);

```

## Example with a class

```Javascript
import { EventInterface } from 'event-interface';


// Implement in a class
class Teapot() {
  
  // We suggest saving event in a private property
  #events;
  constructor() {
    #events = EventInterface.implement(this);
  }
  
  toWarm(temp=20) {
    setTimeout(function(){
    
      this.#events.launch('ready', temp);
      
      // The kettle starts to get cold
      this.#cool();
    
    }.bind(this), temp * 100);
    
  }
  
  #cool(temp) {
   
    setTimeout(function(){
      this.#events.launch('cold', temp);   
    }.bind(this), temp * 50)
    
  }
  
}

// Configure calls
let t = new Teapot();

t.on('ready', function(temp){
  console.log('The tea is ready.', 'Temp:', temp);
});

t.toWarn(30);

await t.when('cold');
console.log('The tea is cold. Heat again');

t.toWarn(50);

```
