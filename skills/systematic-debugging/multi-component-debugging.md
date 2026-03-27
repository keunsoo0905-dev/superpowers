# Multi-Component Debugging

Diagnostic instrumentation for systems with multiple components (CI -> build -> signing, API -> service -> database, etc.).

## Principle

Before proposing fixes in multi-component systems, add diagnostic logging at **every component boundary** to reveal exactly where data flow breaks.

## Instrumentation Template

For EACH component boundary:
1. Log what data enters the component
2. Log what data exits the component
3. Verify environment/config propagation
4. Check state at each layer

Run once to gather evidence showing WHERE it breaks, THEN analyze, THEN investigate that specific component.

## Example: Multi-Layer System (CI/CD Signing Pipeline)

```bash
# Layer 1: Workflow — Are secrets injected?
echo "=== Secrets available in workflow: ==="
echo "IDENTITY: ${IDENTITY:+SET}${IDENTITY:-UNSET}"

# Layer 2: Build script — Does env propagate?
echo "=== Env vars in build script: ==="
env | grep IDENTITY || echo "IDENTITY not in environment"

# Layer 3: Signing script — Is system state correct?
echo "=== Keychain state: ==="
security list-keychains
security find-identity -v

# Layer 4: Actual signing — Does the operation succeed?
codesign --sign "$IDENTITY" --verbose=4 "$APP"
```

**This reveals:** Which layer fails (e.g., secrets -> workflow OK, workflow -> build FAILS).

## Applying to Other Systems

| System Type | Layers to Instrument |
|---|---|
| API pipeline | Request -> middleware -> service -> DB -> response |
| CI/CD | Trigger -> env setup -> build -> test -> deploy |
| Auth flow | Client -> gateway -> auth service -> token store |
| Data pipeline | Source -> transform -> validate -> load -> verify |

For each layer: log inputs, outputs, and config state. One diagnostic run pinpoints the failing boundary.
