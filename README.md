# ReaPackLab Configuration

Centralized repository configuration and synchronization management for **ReaPackLab**, a managed version of [ReaPack](https://codeberg.org/cfillion/reapack) designed for classrooms, computer labs, and other managed REAPER environments.

## Purpose

ReaPackLab allows administrators to centrally manage software distribution across multiple REAPER installations without requiring individual users to configure repositories or initiate updates.

This repository hosts configuration files that determine:

- Which ReaPack repositories are authorized.
- When repository synchronization should occur.
- Which configuration revision is currently active.

All packages from authorized repositories are installed automatically during administrator-directed synchronization.

## Synchronization

ReaPackLab clients automatically check for management instructions:

- **At REAPER startup.**
- **Every five minutes** while REAPER is running.

Clients synchronize only when the administrator has authorized a new synchronization revision.

Administrators can also issue a **Force Sync** instruction through the central configuration system, causing clients to synchronize at their next check even if the repository list hasn't changed.

Computers that are offline receive pending instructions when they reconnect. Operations requiring a REAPER restart may be deferred to avoid interrupting active sessions.

## Administrative Control

Individual users cannot:

- Add, modify, or remove repositories.
- Manually install, update, or uninstall packages through ReaPackLab.
- Initiate synchronization.
- Modify the central management configuration.

## Security

Management instructions will be digitally signed and verified by clients to prevent unauthorized changes.

Private signing keys are never stored in this public repository.

## Related Projects

- **ReaPackLab** — The centrally managed REAPER extension.
- **ReaBootLab** — A planned installer for managed REAPER environments.
- **ReaPack** — The original open-source package manager developed by Christophe Fillon.

## License

This configuration repository is licensed under the MIT License.

The modified ReaPackLab extension is separately governed by ReaPack's applicable LGPL licensing requirements.

## Project Status

ReaPackLab is under development. Configuration formats and synchronization behavior may change.
