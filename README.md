# input
A very simple yet intuitive user input manager.

## Usage:
```javascript
import { Input, Key, MouseButton } from '@h4v0c/input';

const input = new Input();

input.on('key_down', (evt /* KeyEvent */) => console.debug('key_down:', evt));
input.on('key_pressed', (evt /* KeyEvent */) => console.debug('key_pressed:', evt));
input.on('key_up', (evt /* KeyEvent */) => console.debug('key_up:', evt));
input.on('button_down', (evt /* MouseButtonEvent */) => console.debug('button_down:', evt));
input.on('button_clicked', (evt /* MouseButtonEvent */) => console.debug('button_clicked:', evt));
input.on('button_up', (evt /* MouseButtonEvent */) => console.debug('button_up:', evt));
input.on('mouse_move', (evt /* MouseMoveEvent */) => console.debug('mouse_move:', evt));
input.on('mouse_wheel', (evt /* MouseWheelEvent */) => console.debug('mouse_wheel:', evt));

let stop = false;

function loop(time) {
    if (input.isKeyDown(Key.Escape)) {
        stop = true;
        console.debug('loop stopped, stop value:', stop);
    }

    if (input.isKeyDown(Key.Space)) console.debug('SPACE is DOWN');
    if (input.isButtonDown(MouseButton.LEFT)) console.debug('LEFT mouse button is DOWN');

    const arrowDownValue = input.getKeyDownValue(Key.ArrowDown);

    if (arrowDownValue) console.debug('arrowDownValue:', arrowDownValue);

    if (!stop) {
        requestAnimationFrame(loop);
    }
}

requestAnimationFrame(loop);
```

### Output:
```
key_down: {state: 1, key: 34, domEvent: KeyboardEvent}
key_pressed: {state: 0, key: 34, domEvent: KeyboardEvent}
key_up: {state: 0, key: 34, domEvent: KeyboardEvent}

button_down: {state: 1, button: 0, domEvent: MouseEvent}
button_clicked: {state: 0, button: 0, domEvent: MouseEvent}
button_up: {state: 0, button: 0, domEvent: MouseEvent}

mouse_move: { position: Float32Array(2), delta: Float32Array(2), domEvent: MouseEvent }
mouse_wheel: { delta: Float32Array(2), domEvent: WheelEvent }

arrowDownValue: 1
LEFT mouse button is DOWN
SPACE is DOWN

loop stopped, stop value: true
```

***Note:***
In the output above, `Float32Array` refers to the [gl-matrix](https://www.npmjs.com/package/gl-matrix) library `vec2` object, where it's element values are essentially: `[x, y]`. And `domEvent` is the actual DOM event emitted by the DOM.

The `key_down` event supresses subsequent `key_down` events if the key is held down.

The `key_pressed` and `button_clicked` events only get emitted when a `key_up` or `button_up` event is emitted within the `Input.inputThreshold` (milliseconds, default: `200`) since the last `key_down` or `button_down` events have emitted.

    To clarify: If a `key_down` event fires, we store the time. When the `key_up` event fires, we check to see if the time between the two events is less than or equal to the number of milliseconds stored in `Input.inputThreshold`.

    `key_pressed` or `button_clicked` will fire before `key_up` or `button_up`.

`KeyMaps`: This is a feature that will be coming soon...