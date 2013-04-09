jQuery Watchdog
===============

I've quickly become bored writing the same monitoring code in jQuery again and again.  I've also found that binding to `change` is not the most ideal solution (more later).  So, to do things right and in a consistent way I created a watchdog that will let me know when something has changed.

Usage
-----

Find some inputs that you want to monitor and set up a callback that should get fired when they change.

    // Inefficient, but you get the idea
    $('input.firstName, input.lastName').watchdog(function () {
        var first, last;
        first = $('input.firstName').val();
        last = $('input.lastName').val();
        console.log('Your name is now ' + first + ' ' + last);
    });

This will get fired once almost immediately and once every time it changes.

Tricks and Pitfalls
-------------------

This polls every 100ms or so.  That should provide near-instant feedback for users.  Polling is necessary because browsers may fire the `change` event while they are extremely busy.  Your quick-acting code that calls `$('input.firstName').val()` would get the *old* value instead of the new one.  Tragic, horrible, and that's unfortunately how it goes sometimes.

Additional watches may be set by calling `.watchdog()` again and again.

    $('input').watchdog(function () {
        // input.firstName and input.lastName now have two watches
        // and all other inputs on the page have just this one
        console.log('woof woof');  // Detected a change
    });

If you need to progmatically change the value and not trigger the callback, you can just call watchdog() again on the element with no callback function, like this:

    $('input.lastName').val('Johnson').watchdog();  // Skips the callbacks

License
-------

This code is released under an MIT license with a non-advertising clause.  See [LICENSE.md](LICENSE.md)