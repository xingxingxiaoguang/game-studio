# Cocos Creator — Deprecated APIs

*Last verified: 2026-04-19*
*Engine version: 3.8.x*

## v2 APIs Removed in v3

These APIs do NOT exist in Cocos Creator 3.x. Using them will cause errors.

### Namespace & Module System

| Deprecated | Use Instead | Notes |
|------------|-------------|-------|
| `cc.*` global namespace | Named imports: `import { Node, Component } from 'cc'` | All v2 `cc.` references removed |
| `cc.Class({})` | ES6 `class extends Component` with `@ccclass` | v2 class system entirely removed |
| `cc.Polymer()` | ES6 classes | Polymer removed |
| `require('module')` | ES6 `import` statements | CommonJS removed |

### Node & Scene

| Deprecated | Use Instead | Notes |
|------------|-------------|-------|
| `node.runAction(action)` | `tween(node).to(duration, props).start()` | Action system removed |
| `cc.moveTo()`, `cc.scaleTo()`, etc. | `tween()` API | All action helpers removed |
| `cc.sequence()`, `cc.spawn()` | `tween().then()` / parallel tweens | Action composition removed |
| `node.width = X` | `node.getComponent(UITransform).width = X` | Size moved to UITransform |
| `node.height = X` | `node.getComponent(UITransform).height = X` | Size moved to UITransform |
| `node.anchorX = X` | `node.getComponent(UITransform).setAnchorPoint(x, y)` | Anchor moved to UITransform |
| `node.opacity = X` | `node.getComponent(UIOpacity).opacity = X` | Opacity moved to UIOpacity component |
| `node.zIndex = X` | `node.setSiblingIndex(X)` | Sorting changed |

### Lifecycle Methods

| Deprecated | Use Instead | Notes |
|------------|-------------|-------|
| `onLoad()` | `_onLoad()` | Underscore prefix required |
| `start()` | `_start()` | Underscore prefix required |
| `update(dt)` | `_update(dt)` | Underscore prefix required |
| `lateUpdate(dt)` | `_lateUpdate(dt)` | Underscore prefix required |
| `onEnable()` | `_onEnable()` | Underscore prefix required |
| `onDisable()` | `_onDisable()` | Underscore prefix required |
| `onDestroy()` | `_onDestroy()` | Underscore prefix required |

### Resource Loading

| Deprecated | Use Instead | Notes |
|------------|-------------|-------|
| `cc.loader.loadRes()` | `resources.load()` | Entirely different API |
| `cc.loader.loadResDir()` | `resources.loadDir()` | Different API |
| `cc.loader.release()` | `resources.release()` or `assetManager.releaseAsset()` | Different release API |
| `cc.url.raw()` | Direct asset references or `resources.load()` | URL system removed |

### UI System

| Deprecated | Use Instead | Notes |
|------------|-------------|-------|
| `cc.ScrollView` | `ScrollView` with Layout + Widget | Same concept, new import |
| `cc.Layout` with `cc.Vertical`/`cc.Horizontal` | `Layout` with `Type.VERTICAL`/`Type.HORIZONTAL` | Enum values changed |
| `cc.EditBox` | `EditBox` component | Same concept, new import |
