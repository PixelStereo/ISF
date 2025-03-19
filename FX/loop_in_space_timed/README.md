# Loop In Space timed

- [Loop In Space timed](#loop-in-space-timed)
  - [ISF Inputs](#isf-inputs)
    - [Image](#image)
    - [Clock](#clock)
    - [Flip Horizontally](#flip-horizontally)
    - [Flip Vertically](#flip-vertically)
    - [Enable Zoom](#enable-zoom)
    - [Zoom](#zoom)
  - [Running Millumin 5 example](#running-millumin-5-example)
  - [TODO](#todo)

## ISF Inputs

### Image

Effect's `inputImage`.

- Type: `image`.

### Clock

User-provided continuous time to translate the texture.

To prevent 'jumps' in image translation when changing translation speed, the effect avoid internal `TIME`, that we would have to multiply by a `speed` input which would actually modify time itself (like seeking in a media).

Instead, the effect expects an external time that can be passed by a function like this one (example in Javascript / pseudo-code):

```javascript
// First clock value.
const clock = [0, 0];

// First speed value.
const speed = [0, 0];

// Change speed, could be received via OSC.
onSpeedChange = (value) => {
  speed[0] = value.x;
  speed[1] = value.y;
};

// Increment clock continuously.
const fps = 60;
const interval = 1000 / fps;

setInterval(() => {
  clock[0] += interval * speed[0];
  clock[1] += interval * speed[1];

  // Set ISF input, could be sent via OSC.
  setEffectInput('clock', clock);
}, interval);
```

See Millumin 5 example script [here](./Millumin/TO%20COPY%20IN%20USER%20LIBRARY/Scripts/LoopInSpaceClock.js).

### Flip Horizontally

Alternatively flip texture on `x`.

- Type: `bool`.
- Default: `false`.

### Flip Vertically

Alternatively flip texture on `y`.

- Type: `bool`.
- Default: `false`.

### Enable Zoom

- Type: `bool`.
- Default: `false`.

### Zoom

- Type: `Point2d` in **Percents**.
- Default: `[100, 100]`.
- Min: `[0, 0]`.
- Max: `[1000, 1000]`.

## Running Millumin 5 example

Example uses new Millumin features:

- Javascript.
- Internal Signal.

Copy files:

- Go to [Millumin composition's resources folder](./Millumin//TO%20COPY%20IN%20USER%20LIBRARY/).
- Copy `IFF-effects/PixelStereo` in `/Users/[YOUR_NAME]/Library/Millumin/ISF-effects`
- Copy `Scripts/LoopInSpaceClock.js`in `/Users/[YOUR_NAME]/Library/Millumin/Scripts`

Run [`LoopInSPaceTimed.millumin`](./Millumin//LoopInSpaceTimed.millumin) in Millumin 5.

## TODO

- [ ] Use original `TIME` and create a `speed` input.
  - [ ] Explore creating buffer encoding last computed time? (see [here](https://stackoverflow.com/questions/34963366/encode-floating-point-data-in-a-rgba-texture))
  - [ ] Explore `TIMEDELTA` and `FRAMEINDEX` to get the right math for our purpose.
