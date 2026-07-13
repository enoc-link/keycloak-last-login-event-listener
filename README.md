# Keycloak Last Login Event Listener

A Keycloak SPI plugin that tracks last login time for each user by storing timestamps as custom user attributes.

## How it works

On every successful `LOGIN` event, the plugin:

- Copies the existing `last-login` value into `prior-login` (preserving the previous session's timestamp)
- Writes `last-login` — ISO 8601 UTC string (e.g. `2026-07-13T10:30:00Z`)
- Writes `last-login-timestamp` — raw epoch milliseconds as a string

Admin events are intentionally ignored.

## Requirements

- Java 21
- Gradle 8.14.1
- Keycloak 26.6.1

## Build

```bash
JAVA_HOME=/path/to/java-21 ./gradlew build
```

The JAR will be produced at `build/libs/`.

> **Note:** Gradle 8.14.1 supports up to Java 21. If your system default JVM is newer, set `JAVA_HOME` explicitly to a Java 21 installation before building.

## Installation

1. Copy the JAR into your Keycloak `providers/` directory.
2. Restart Keycloak.
3. In the Keycloak admin UI, go to **Events → Config** and add `last_login` to the event listeners list.

## User attributes set

| Attribute | Type | Description |
|---|---|---|
| `last-login` | ISO 8601 UTC string | Timestamp of the most recent login |
| `last-login-timestamp` | String (epoch ms) | Timestamp of the most recent login as epoch milliseconds |
| `prior-login` | ISO 8601 UTC string | Timestamp of the previous login |

## Credits

Based on the templates provided by [zonaut](https://github.com/zonaut/keycloak-extensions).
