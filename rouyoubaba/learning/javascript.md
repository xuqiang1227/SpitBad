#What is event.originalEvent?

It's also important to note that the event object contains a property called originalEvent, which is the event object that the browser itself created. jQuery wraps this native event object with some useful methods and properties, but in some instances, you'll need to access the original event via event.originalEvent for instance. This is especially useful for touch events on mobile devices and tablets.

[event.originalEvent jQuery](http://stackoverflow.com/questions/16674963/event-originalevent-jquery)
