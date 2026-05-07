# Plugin System

## Purpose
Guidance for projects that require extensibility through a plugin or module system.

## Use When
- Extensibility is a primary product requirement
- Third parties or users will add functionality
- The core system needs to be decoupled from its extensions

## Rules
- Use registries for extensible systems — plugins register themselves, the core discovers them.
- Define stable interfaces — a plugin contract that does not change between versions.
- Keep plugin boundaries explicit — plugins should not reach into core internals.
- Core system must work without any plugins installed.
- Version the plugin API.

## Patterns
- Plugin interface: a TypeScript interface that every plugin must implement.
- Registry: a singleton that plugins register with at startup.
- Core calls plugins through the interface, never directly.

```ts
interface Plugin {
  id: string;
  name: string;
  init(context: PluginContext): void;
}

const registry = new PluginRegistry();
registry.register(myPlugin);
```

## Anti-Patterns
- Plugins importing from core internals directly.
- No versioning on the plugin interface (breaking changes silently).
- Tightly coupling the plugin lifecycle to the core initialization.

## Related Files
- decisions/architecture-choice.md
