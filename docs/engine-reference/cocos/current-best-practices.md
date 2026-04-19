# Cocos Creator — Current Best Practices

*Last verified: 2026-04-19*
*Engine version: 3.8.x*

## TypeScript Configuration

- Use `"strict": true` in `tsconfig.json`
- Enable `"noImplicitAny": true`
- Use ES2020+ target for modern TypeScript features
- Cocos Creator 3.x natively uses TypeScript — never use plain JavaScript

## Component Architecture

### Decorators

```typescript
import { _decorator, Component, Node, SpriteFrame, Color } from 'cc';
const { ccclass, property } = _decorator;

@ccclass('HeroController')
export class HeroController extends Component {
    @property({ type: Node })
    targetNode: Node = null!;

    @property({ type: Number })
    moveSpeed: number = 100;

    // Use slider for designer-friendly ranges
    @property({ type: Number, range: [0, 100, 1], slide: true })
    healthPercent: number = 100;
}
```

### Lifecycle

```typescript
@ccclass('MyComponent')
export class MyComponent extends Component {
    // Cache references here — safe to access other nodes
    _onLoad() {
        this._sprite = this.node.getComponent(Sprite)!;
    }

    // Called every frame — keep lean
    _update(dt: number) {
        // Use dt, never assume fixed timestep
        this.node.setPosition(
            this.node.position.x + this.moveSpeed * dt,
            this.node.position.y,
            this.node.position.z
        );
    }

    // Subscribe to events, enable timers
    _onEnable() {
        this.node.on(Node.EventType.TOUCH_START, this._onTouchStart, this);
    }

    // Unsubscribe — prevent memory leaks
    _onDisable() {
        this.node.off(Node.EventType.TOUCH_START, this._onTouchStart, this);
    }

    // Final cleanup
    _onDestroy() {
        // Return to pool, release references
    }
}
```

## Performance (Mobile-First)

### Object Pooling

```typescript
// Use NodePool for frequently created/destroyed objects
const pool = new NodePool('Bullet');

// Get from pool
getBullet(): Node {
    if (pool.size() > 0) {
        return pool.get()!;
    }
    return instantiate(this.bulletPrefab);
}

// Return to pool
returnBullet(bullet: Node) {
    pool.put(bullet);
}
```

### Draw Call Optimization

- Use Auto Atlas to batch UI sprites into shared textures
- Minimize unique materials — shared materials batch together
- Keep UI draw calls under 10 per frame on mobile
- Use `StaticBatching` for static scene elements
- Limit particle systems — each active particle adds draw call overhead

### Memory Management

- Release unused assets via `assetManager.releaseAsset()`
- Unload scenes with `director.purgeScene()` when transitioning
- Pool nodes instead of instantiate/destroy cycles
- Monitor with Cocos Creator's built-in profiler

## Mobile Touch Input

```typescript
import { EventTouch, Node } from 'cc';

// Register touch events
this.node.on(Node.EventType.TOUCH_START, this._onTouchStart, this);
this.node.on(Node.EventType.TOUCH_MOVE, this._onTouchMove, this);
this.node.on(Node.EventType.TOUCH_END, this._onTouchEnd, this);
this.node.on(Node.EventType.TOUCH_CANCEL, this._onTouchCancel, this);

private _onTouchStart(event: EventTouch) {
    const location = event.getUILocation();
    // Minimum 44pt touch target for mobile
}
```

## Async Resource Loading

```typescript
import { resources, SpriteFrame, AssetManager } from 'cc';

// Load single asset
loadSprite(path: string): Promise<SpriteFrame> {
    return new Promise((resolve, reject) => {
        resources.load(path, SpriteFrame, (err, asset) => {
            if (err) { reject(err); return; }
            resolve(asset);
        });
    });
}

// Use with async/await
async _initAssets() {
    try {
        this.heroSprite = await this.loadSprite('heroes/tank-sprite');
    } catch (err) {
        console.error('Failed to load asset:', err);
    }
}
```

## Tween Animation

```typescript
import { tween, Vec3 } from 'cc';

// Basic tween (replaces v2 action system)
tween(this.node)
    .to(0.5, { position: new Vec3(100, 0, 0) }, { easing: 'quadOut' })
    .to(0.3, { scale: new Vec3(1.2, 1.2, 1.2) })
    .call(() => { this._onAnimationComplete(); })
    .start();

// Sequence with delay
tween(this.node)
    .delay(0.2)
    .to(0.5, { position: targetPos })
    .start();
```

## Common Mistakes

1. **Using v2 `cc.` namespace** — Import from `'cc'` module instead
2. **Forgetting `_onLoad` cache** — Calling `getComponent()` every frame is expensive
3. **Not removing event listeners** — Always pair `on()` with `off()` in `_onDisable()`
4. **Missing `!` on properties** — Use non-null assertion for `_onLoad`-initialized fields
5. **Using `any` type** — Use `unknown` if type is uncertain
6. **Synchronous loading** — Always use async patterns for resource loading
7. **Not pooling nodes** — Instantiate/destroy creates GC pressure on mobile
