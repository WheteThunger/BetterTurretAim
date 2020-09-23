**Better Turret Aim** improves the speed at which auto turrets aim at their current target.

In vanilla Rust, a player sprinting on a horse can avoid being shot by an auto turret they are passing because the turret cannot follow them quickly enough. This plugin addresses this problem by allowing turrets to follow their current target more quickly. This also works well for turrets deployed to vehicles, since moving past a stationary target at a moderate pace will usually cause the turret to not shoot at all.

Notes:
- This plugin does not increase the auto turret target lock speed, so a player or object moving really fast may be able to quickly enter and exit the turret's range without being shot at because the turret never locked onto them.
- This plugin does not affect animation, so the auto turret may appear to be aiming behind the target it's shooting if the aiming speed is configured very high.
- Does not affect NPC auto turrets because they already have fast aim.

## Permissions

- `betterturretaim.use` -- Improves the aiming ability of auto turrets owned by players who have been granted this permission. Has no effect unless the configuration option `RequirePermission` is `true`.

## Configuration

Default configuration:

```json
{
  "RequirePermission": true,
  "OnlyVehicles": false,
  "UpdateIntervalSeconds": 0.015,
  "Interpolation": 0.5
}
```

Note: Vanilla rust updates auto turret aim every `0.015` seconds. Each update alters the aim to cover approximately 90% of the gap distance. This plugin works by supplementing the vanilla aiming updates with additional aiming updates. These additional updates are optimized and only apply while the auto turret has a target, so this is unlikely to significantly impact performance.

- `RequirePermission` (`true` or `false`) -- While `true`, only auto turrets owned by players with the `betterturretaim.use` permission will be affected.
- `OnlyVehicles` (`true` or `false`) -- While `true`, only auto turrets attached to vehicles (e.g., minicopters, modular cars) will be affected.
- `Interpolation` (`0.0` - `1.0`) -- This affects how much the auto turret's aim will be adjusted from its current point to the target point, per update. Setting to `1.0` will allow the turret to fire on its target continuously (assuming no obstacles are in the way). Since this plugin supplements vanilla aim which uses an interpolation value of roughly `0.9`, you can see a noticeable improvement from a low value such as `0.25`, so don't feel the need to raise this very much if you want a semblance of balance.
- `UpdateIntervalSeconds` -- This affects how often the auto turret's aim is updated. Performance-conscious users can increase this value to reduce the update frequency. Increasing this should probably be accompanied by an increase in `Interpolation` to maintain overall aiming speed. For example, using a value of `0.1` and an `Interpolation` value of `1.0`, the auto turret can fire *at most* 10 bullets per second at a fast moving target, but fire rate will likely be lower because it's only maximized when the updates happen to align with the weapon's firing rate.

## Developer Hooks

#### OnAutoTurretAimImprove

- Called when this plugin is about to improve the aim of an auto turret. This is usually when the plugin loads, when an auto turret spawns, and when the plugin's permissions are granted or revoked.
- Returning `false` will prevent the auto turret from being affected.
- Returning `null` will result in the default behavior.

```csharp
object OnAutoTurretAimImprove(AutoTurret autoTurret)
```
