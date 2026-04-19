# Cocos Creator — Breaking Changes

*Last verified: 2026-04-19*
*Engine version: 3.8.x*

## v2.x → v3.x (MAJOR migration)

The v2→v3 transition was a complete architectural rewrite. v2 APIs do NOT work in v3.

### Core Architecture

| v2 API | v3 Replacement | Impact |
|--------|---------------|--------|
| `cc.` namespace | Individual module imports (`import { Node } from 'cc'`) | HIGH — all imports changed |
| `cc.Class()` | ES6 classes with `@ccclass` decorator | HIGH — no more extend syntax |
| `cc.js.setClassName()` | `@ccclass('ClassName')` decorator | HIGH — class registration changed |
| `cc.Enum()` | TypeScript `enum` | MEDIUM — use native enums |
| `cc._RF.push()` | Module system | HIGH — file structure changed |

### Scene & Node

| v2 API | v3 Replacement | Impact |
|--------|---------------|--------|
| `node.runAction()` | `tween(node).to()` / `tween(node).by()` | HIGH — action system replaced with tween |
| `node.setScale()` | `node.setScale(x, y, z)` (3D scale) | MEDIUM — now requires z parameter |
| `node.position` | `node.setPosition()` / `node.position` getter | MEDIUM — Vec3 instead of Vec2 |
| `node.width` / `node.height` | `node.getComponent(UITransform).width` | HIGH — size is now on UITransform component |
| `node.anchorX` / `node.anchorY` | `node.getComponent(UITransform).setAnchorPoint()` | HIGH — anchor is now on UITransform |
| `node.color` | `node.getComponent(UIOpacity)` or Sprite color | MEDIUM — opacity separated to component |
| `node.opacity` | `node.getComponent(UIOpacity).opacity` | HIGH — opacity is now a separate component |
| `node.zIndex` | `node.setSiblingIndex()` | MEDIUM — sorting changed |

### Components

| v2 API | v3 Replacement | Impact |
|--------|---------------|--------|
| `properties: {}` | `@property({ type: Type })` decorator | HIGH — all property declarations changed |
| `onLoad()` | `_onLoad()` (underscore prefix) | MEDIUM — lifecycle methods renamed |
| `start()` | `_start()` | MEDIUM |
| `update(dt)` | `_update(dt)` | MEDIUM |
| `onEnable()` / `onDisable()` | `_onEnable()` / `_onDisable()` | MEDIUM |
| `onDestroy()` | `_onDestroy()` | MEDIUM |

### Rendering

| v2 API | v3 Replacement | Impact |
|--------|---------------|--------|
| `cc.Sprite` sprite frames | `Sprite.spriteFrame` (same concept, new API) | MEDIUM |
| `cc.Label` | `Label` with `@ccclass` | LOW — mostly compatible |
| `cc.Button` | `Button` component (same, with TypeScript) | LOW |
| Custom shaders via `.effect` | Effect file format (`.effect`) | MEDIUM — syntax updated |

### Resource Loading

| v2 API | v3 Replacement | Impact |
|--------|---------------|--------|
| `cc.resources.load()` | `resources.load()` | MEDIUM — no `cc.` prefix |
| `cc.loader.loadRes()` | `resources.load()` | HIGH — entirely different API |
| `cc.assetManager.loadRemote()` | `assetManager.loadRemote()` | MEDIUM — no `cc.` prefix |

## 3.7 → 3.8 Changes

| Area | Change | Impact |
|------|--------|--------|
| Rendering | Improved batch rendering for 2D | LOW — mostly transparent improvement |
| Physics | Updated physics engine versions | LOW — same API surface |
| UI | New UI components and improvements | LOW — additive changes |
| Build | Updated build pipeline | MEDIUM — check build configurations |
