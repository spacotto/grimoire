# Building Flexible Systems in Python

Flexible systems are designed to evolve. Rather than hardcoding behaviour, they expose well-defined extension points, load logic at runtime, and adapt to change through configuration — not rewrites.

This document covers the core patterns Python developers use to build software that stays maintainable as requirements shift: plugin architectures, hot-swappable components, configuration-driven design, dynamic registration, and versioning strategies.

## Plugin Architectures

A plugin architecture lets you add functionality without modifying core code. The system defines an interface; plugins implement it.

```python
# Base interface all plugins must follow
from abc import ABC, abstractmethod

class ExporterPlugin(ABC):
    @abstractmethod
    def export(self, data: dict) -> str:
        """Convert data to a target format."""
        ...

# Plugin implementations
class JSONExporter(ExporterPlugin):
    def export(self, data: dict) -> str:
        import json
        return json.dumps(data, indent=2)

class CSVExporter(ExporterPlugin):
    def export(self, data: dict) -> str:
        headers = ",".join(data.keys())
        values = ",".join(str(v) for v in data.values())
        return f"{headers}\n{values}"
```

>[!NOTE]
>Plugins are discovered and loaded at runtime — the core system never needs to know about them in advance.

>[!TIP]
>**Key principle:** depend on abstractions (ABCs or protocols), not concrete classes.

## Extensible System Design

An extensible system is open for extension, closed for modification (the Open/Closed Principle). New behaviour is added by writing new code, not by editing existing code.

```python
from typing import Callable

class Pipeline:
    def __init__(self):
        self._steps: list[Callable] = []

    def add_step(self, fn: Callable) -> "Pipeline":
        self._steps.append(fn)
        return self  # enables chaining

    def run(self, data):
        for step in self._steps:
            data = step(data)
        return data

# Extending the pipeline without touching its internals
def strip_whitespace(text: str) -> str:
    return text.strip()

def to_uppercase(text: str) -> str:
    return text.upper()

pipeline = Pipeline()
pipeline.add_step(strip_whitespace).add_step(to_uppercase)

result = pipeline.run("  hello world  ")
# → "HELLO WORLD"
```

## Hot-Swappable Components

Hot-swappable components can be replaced at runtime — without restarting the system. This is useful for switching strategies, backends, or providers dynamically.

```python
class NotificationService:
    def __init__(self, sender):
        self._sender = sender

    def swap(self, new_sender) -> None:
        """Replace the active sender at runtime."""
        self._sender = new_sender

    def notify(self, message: str) -> None:
        self._sender.send(message)


class EmailSender:
    def send(self, message: str) -> None:
        print(f"[Email] {message}")

class SMSSender:
    def send(self, message: str) -> None:
        print(f"[SMS] {message}")


service = NotificationService(EmailSender())
service.notify("System started")       # [Email] System started

service.swap(SMSSender())
service.notify("Alert: high CPU")      # [SMS] Alert: high CPU
```

>[!NOTE]
>The caller never changes — only the injected component does.

## Configuration-Driven Behaviour

Instead of hardcoding behaviour, read it from configuration. This separates *what the system does* from *how it is deployed*.

```python
import json
from pathlib import Path

# config.json
# {
#   "log_level": "INFO",
#   "max_retries": 3,
#   "exporter": "json"
# }

class AppConfig:
    def __init__(self, path: str):
        raw = json.loads(Path(path).read_text())
        self.log_level: str = raw.get("log_level", "WARNING")
        self.max_retries: int = raw.get("max_retries", 1)
        self.exporter: str = raw.get("exporter", "json")

    def __repr__(self):
        return (
            f"AppConfig(log_level={self.log_level!r}, "
            f"max_retries={self.max_retries}, "
            f"exporter={self.exporter!r})"
        )

config = AppConfig("config.json")
```

A factory then maps config values to concrete implementations:

```python
EXPORTERS = {
    "json": JSONExporter,
    "csv": CSVExporter,
}

def get_exporter(config: AppConfig) -> ExporterPlugin:
    cls = EXPORTERS.get(config.exporter)
    if cls is None:
        raise ValueError(f"Unknown exporter: {config.exporter!r}")
    return cls()
```

## Dynamic Feature Registration

A registry lets components register themselves, so the core system discovers them without explicit imports.

```python
class FeatureRegistry:
    def __init__(self):
        self._features: dict[str, Callable] = {}

    def register(self, name: str):
        """Decorator to register a feature by name."""
        def decorator(fn: Callable) -> Callable:
            self._features[name] = fn
            return fn
        return decorator

    def get(self, name: str) -> Callable:
        if name not in self._features:
            raise KeyError(f"Feature {name!r} not registered.")
        return self._features[name]

    def list_features(self) -> list[str]:
        return list(self._features.keys())


registry = FeatureRegistry()

@registry.register("greet")
def greet(name: str) -> str:
    return f"Hello, {name}!"

@registry.register("farewell")
def farewell(name: str) -> str:
    return f"Goodbye, {name}!"

fn = registry.get("greet")
print(fn("Silvia"))  # Hello, Silvia!
```

>[!NOTE]
>Plugins can register themselves on import — no central list needed.

## Versioning and Compatibility

Version your APIs explicitly so consumers know what to expect.

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Version:
    major: int
    minor: int
    patch: int

    def __str__(self) -> str:
        return f"{self.major}.{self.minor}.{self.patch}"

    def is_compatible_with(self, other: "Version") -> bool:
        """Same major version = backward-compatible (semver convention)."""
        return self.major == other.major


class APIHandler:
    VERSION = Version(2, 1, 0)

    def handle(self, request_version: Version, payload: dict) -> dict:
        if not self.VERSION.is_compatible_with(request_version):
            raise ValueError(
                f"Incompatible version: handler={self.VERSION}, "
                f"request={request_version}"
            )
        return {"status": "ok", "data": payload}
```

Use [semantic versioning](https://semver.org): `MAJOR.MINOR.PATCH`.
- **MAJOR** — breaking change
- **MINOR** — new feature, backward-compatible
- **PATCH** — bug fix

## Migration Strategies

When a data format or API changes, migrations translate old data to the new shape.

```python
def migrate_v1_to_v2(record: dict) -> dict:
    """v1 used 'username'; v2 uses 'user_name'."""
    record = record.copy()
    if "username" in record:
        record["user_name"] = record.pop("username")
    record["schema_version"] = 2
    return record

MIGRATIONS = {
    1: migrate_v1_to_v2,
    # 2: migrate_v2_to_v3, ...
}

def migrate(record: dict, target_version: int) -> dict:
    current = record.get("schema_version", 1)
    while current < target_version:
        fn = MIGRATIONS.get(current)
        if fn is None:
            raise ValueError(f"No migration from version {current}")
        record = fn(record)
        current = record.get("schema_version", current + 1)
    return record


old_record = {"username": "silvia", "schema_version": 1}
new_record = migrate(old_record, target_version=2)
# → {"user_name": "silvia", "schema_version": 2}
```

>[!TIP]
>Keep migrations **pure functions** (no side effects) and **composable** (each handles one step).

## Backward Compatibility

Backward compatibility means old consumers still work after a change. Three practical rules:

**1. Never remove — deprecate first.**

```python
import warnings

def get_user(user_id: int, username: str = None):
    if username is not None:
        warnings.warn(
            "'username' is deprecated, use 'user_id' only.",
            DeprecationWarning,
            stacklevel=2,
        )
    # ... fetch by user_id
```

**2. Use `**kwargs` to absorb unknown future fields.**

```python
def process_event(event_type: str, payload: dict, **kwargs):
    # kwargs absorbs fields added in future versions
    print(f"Processing {event_type}: {payload}")
```

**3. Provide defaults for new required fields.**

```python
def build_response(data: dict, version: int = 1, meta: dict = None) -> dict:
    return {
        "version": version,
        "data": data,
        "meta": meta or {},
    }
```

>[!NOTE]
>Adding `version` and `meta` with defaults keeps all existing call sites working unchanged.

>[!TIP]
>**Rule of thumb:** design for the change you expect, not every change imaginable. Flexibility has a cost — apply these patterns where variability is real.
