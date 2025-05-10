# anime.js Reference

## Core Concepts
- **Version**: 4.0.0 (24.5KB total, modular imports available)
- **Main Function**: `animate(targets, properties)`
- **Targets**: CSS selectors, DOM elements, JS objects, or arrays
- **Properties**: CSS props, transforms, SVG attributes, object properties

## Module Imports
```javascript
import { animate } from 'animejs';
import { createTimeline, createScope, createDraggable, createSpring } from 'animejs';
import { stagger, morphTo, createMotionPath, createDrawable, onScroll } from 'animejs';
```

## Basic Animation
```javascript
animate('.element', {
  translateX: 250,            // Simple value
  opacity: [0, 1],            // From -> To
  scale: { value: 2, delay: 200 },  // With parameters
  rotate: '+=1turn',          // Relative value
  duration: 1000,
  easing: 'easeOutElastic(1, .5)',
  loop: true
});
```

## Animation Types

### Timeline
```javascript
const timeline = createTimeline();
timeline
  .add('.el1', { translateX: 250 })
  .add('.el2', { translateX: 250 }, '-=600') // Start 600ms before previous ends
  .add('.el3', { translateX: 250 }, '+=200'); // Start 200ms after previous ends
```

### Staggering
```javascript
animate('.elements', {
  translateY: 100,
  delay: stagger(100),  // 100ms between each
  backgroundColor: stagger(['#FF0000', '#00FF00', '#0000FF']), // Values
  translateX: stagger(10) // Each element moves 10px more
});

// Grid stagger
animate('.grid', {
  scale: [0, 1],
  delay: stagger(50, { grid: [5, 5], from: 'center', axis: 'x' })
});
```

### SVG Animation
```javascript
// Line drawing
animate(createDrawable('path'), { draw: [0, 1] });

// Shape morphing 
animate('path', { d: morphTo('path2') });

// Motion path
animate('.element', { ...createMotionPath('path') });
```

### Scroll-Based
```javascript
// Trigger on scroll
animate('.element', {
  translateY: [100, 0],
  autoplay: onScroll()
});

// Sync with scroll position
animate('.element', {
  translateY: [0, 100],
  autoplay: onScroll({ sync: true, threshold: [0, 0.5] })
});
```

### Draggable
```javascript
createDraggable('.element', {
  container: '.container',
  snapX: [0, 100, 200],
  releaseStiffness: 100,
  releaseDamping: 10
});
```

### Responsive
```javascript
createScope({
  mediaQueries: {
    mobile: '(max-width: 768px)',
    desktop: '(min-width: 769px)'
  }
}).add(({ matches }) => {
  if (matches.mobile) {
    animate('.element', { translateX: 50 });
  } else {
    animate('.element', { translateX: 250 });
  }
});
```

## Animation Control
```javascript
const anim = animate('.element', { translateX: 250, autoplay: false });

// Methods
anim.play();
anim.pause();
anim.restart();
anim.reverse();
anim.seek(500);
anim.complete();

// Promise-based
anim.play().then(() => console.log('Animation finished'));
```

## Common Parameters
```javascript
animate('.element', {
  // Timing
  duration: 1000,            // Duration in ms
  delay: 500,                // Delay before start
  endDelay: 500,             // Delay after finish
  
  // Playback
  loop: 3,                   // Number of loops (true = infinite)
  alternate: true,           // Reverse on each loop
  autoplay: true,            // Auto-start animation
  loopDelay: 1000,           // Delay between loops
  reversed: false,           // Start reversed
  
  // Easing
  easing: 'easeOutQuad',     // Named easing
  // Other easings: linear, spring(1,80,10,0), steps(5), cubicBezier(.5,0,.5,1)
  
  // Composition
  composition: 'blend',      // For transforms: 'replace' or 'blend'
  
  // Callbacks
  onBegin: (anim) => {},
  onUpdate: (anim) => {},
  onComplete: (anim) => {}
});
```

## Best Practices
- Animate `transform` and `opacity` for performance
- Import only needed modules to minimize bundle size
- Use WAAPI for hardware acceleration (auto-used for transforms/opacity)
- For SVG manipulation, ensure valid path data
- Set `debug: true` for troubleshooting

## Common Animation Recipes

**Fade In/Out**: `opacity: [0, 1]`
**Slide**: `translateX: [-100, 0]`
**Grow/Shrink**: `scale: [0, 1]`
**Bounce**: `translateY: [0, -30, 0]`
**Rotate**: `rotate: '1turn'`
**Type Effect**: `delay: stagger(50)` on letters
**Color Change**: `backgroundColor: ['#FF0000', '#0000FF']`
**Loading Spinner**: `rotate: '1turn', loop: true`
