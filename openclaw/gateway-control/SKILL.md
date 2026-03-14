---
name: gateway-control
description: Manage and restart the OpenClaw Gateway service. Use when the gateway is unstable, after config changes that require a full restart, or when the user explicitly requests a service restart.
---

# Gateway Control

This skill provides tools and procedures to manage the OpenClaw Gateway systemd service.

## Workflow

1.  **Check Status**: Verify the current state of the gateway service.
    ```bash
    systemctl status openclaw.service
    ```
2.  **Restart Service**: When needed, perform a full service restart.
    ```bash
    sudo systemctl restart openclaw.service
    ```
3.  **Verify**: Confirm the service is back up and running.

## When to use
- Gateway disconnections that persist after reconnect attempts.
- Changes to `openclaw.json` that require a full process restart (e.g., port changes, certain plugin enables).
- When the model notices the gateway is in a degraded state.