# AutoRetainer Cache Refresh

InventoryReporter can optionally use AutoRetainer IPC to refresh retainer inventory caches.

## Flow

- Open a summoning bell and show the retainer list.
- Press `Refresh Retainer Cache with AutoRetainer` in InventoryReporter, or use the matching button drawn in AutoRetainer's retainer-list overlay.
- AutoRetainer visits each available retainer.
- InventoryReporter opens `Entrust or withdraw items`, lets the existing retainer inventory close hook refresh the cache, then releases AutoRetainer's postprocess lock.
- After the full batch finishes, InventoryReporter sends one report.

## Integration Boundary

This does not reference `AutoRetainerAPI.dll`; it uses AutoRetainer's existing IPC names directly. If AutoRetainer is not installed or loaded, InventoryReporter's existing manual cache behavior is unchanged. InventoryReporter only requests AutoRetainer retainer postprocess work during an explicit full cache refresh started by the user.

## Automation Rules

- Treat AutoRetainer postprocess callbacks as scheduling signals, not proof that the game UI is ready.
- Confirm the active retainer session or actionable command menu before selecting anything.
- Select the localized `Entrust or withdraw items` command by text, not by fixed index.
- Run addon and object-table reads on the framework thread.
- Include the current retainer UI state in timeout errors so automation issues remain diagnosable.
