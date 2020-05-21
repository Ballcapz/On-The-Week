# JavaScript Modules and Closures

## What is a closure

In general, a closure is a function that has `closure` over another function. Which means that that if function A has closure over function B, function B can't finish until function A completes execution.

In the simple case we are using in JavaScript here, we are using `closure` to keep from polluting the global namespace, and make for better code seperation and code understandability.

Here is an example taken from a simple game:

```js
// Player in the game
function Player(initialPosition) {

    let playerObject = {
        x: 0,
        y: 0,
        direction: 'up',
        setX: function(val) {
            this.x = val;
        },
        setY: function(val) {
            this.y = val;
        },
        move: function() {
            case direction:
                'up': ...
                // removed for brevity
        }
    };

    // setting values based on what is passed in
    playerObject.x = initialPosition.x;
    playerObject.y = initialPosition.y;

    return playerObject;

}

```

In general terms, we can refer to this pattern as a closure, with the playerObject having closure over the function `Player`. Here is an example of how it might be used:

```js
// where we want to use the Player and it's functionality
const player = Player({x: 10, y: 10});

player.move();

player.direction = 'down';

// any other interaciton with the player...
```

This is all well and good now, but it becomes even more powerful and useful when combined with modules

## Introducing Modules

Lets look at an example of using modules along with how we were previously using closures:

```js
// in player.js

export function Player(initialPosition) {

    let playerObject = {
        // ...
    };

    return playerObject;
}

```

And then the use of it in a different file:

```js
// in main.js
import {Player} from './player.js';

// IIFE to fun on load, and generally scope our use of Player()
(function () {
    const player = Player({x: 10, y: 10});

    player.move();

    player.direction = 'down';

    // fun player things...
})()

```


This gives us a nice way to practice some seperation of concerns, and leads to more understandable code that becomes easier to maintain and reason about.
It is also quite extensible, with only having to include a single file in the html page you want this included on. The rest of the dependencies get resolved in the background through the import/export statements.

This is currently supported in all major browsers via script tag except in IE.
[Can I Use](https://caniuse.com/#search=modules).

## Using Modules and Closures

In order to use it, you need to include the reference to the main file somewhere. Say you want `main.js` and all of its' dependencies on `index.html`, include it at the bottom of the body:

```html
<body>
<!-- other stuff on the page -->

<script type="module" src="./main.js"></script>

</body>
```

Then main.js will be included and used as normal on `index.html`.

## Other considerations

### Local Development
When developing, the browser using the `file:\\` protocol can't resolve the import/export statements. So, it needs to be served from some sort of server.
One quick way to do this in development is using `npx serve` in the same directory as your main entrypoint that is included on the html page.