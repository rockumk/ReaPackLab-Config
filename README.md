# ReaPackLab — Central Configuration

This repository holds the centrally managed configuration for
[ReaPackLab](https://github.com/rockumk/ReaPackLab), a classroom/lab fork of
[ReaPack](https://reapack.com/). Classroom computers read one hosted INI
instead of asking students to manage repositories or update packages.

## Open the administrator utility

**[Open ReaPackLab Admin — live web utility](https://rockumk.github.io/ReaPackLab-Config/)**

[Direct link to the utility](https://rockumk.github.io/ReaPackLab-Config/ReaPackLab_Admin.html)
· [View its HTML/JavaScript source](ReaPackLab_Admin.html)

The utility runs entirely in your browser. It does not need a GitHub login,
send the teacher password to a server, edit the repository automatically, or
install packages. You can also download the HTML file and open it locally.

### Creating or updating the configuration

1. Open **ReaPackLab Admin** using the live link above.
2. Select **Import ReaPackLab.ini** and load the existing
   [ReaPackLab.ini](ReaPackLab.ini). This preserves the existing salted
   teacher-password verifier. Leave the password boxes blank unless you want
   to change the password.
3. Edit **list_version**, **sync_version**, the multiline repository list,
   and the **Obsolete packages** option.
4. Click **Validate repositories**, then **Generate configuration**.
   Examine the generated text.
5. **Copy** or **Download ReaPackLab.ini**, then manually commit the updated
   text/file to this repository as `ReaPackLab.ini` on the `main` branch.
   The utility does not publish the file to GitHub.

**Deployment caution:** Do not publish `xURL`, `-URL` in its new meaning,
or an `[Options]` section until the computers reading this file have been
upgraded to a version that supports them. Older versions interpreted `-URL`
as unsubscribe-only and may reject `xURL` or `[Options]`. Roll out and
test the new client before publishing a new-format configuration.

## What ReaPackLab reads

The active configuration is **[ReaPackLab.ini](ReaPackLab.ini)**, available to
the REAPER extension at:

```text
https://raw.githubusercontent.com/rockumk/ReaPackLab-Config/main/ReaPackLab.ini
```

Each managed Mac has a local file
`/Library/Application Support/ReaPackLab/url.txt` containing that
HTTPS URL (one line only). The hosted INI contains the revisions, teacher
password verifier, options, and repository instructions.

The earlier **`repositories.txt`** and **`sync.txt`** formats were
superseded by this single INI and have been deleted from the repository.

### Versions and synchronization

| Setting | Purpose |
| --- | --- |
| `list_version` | Increase when changing the approved repository list or its directives. |
| `sync_version` | Increase to request a managed synchronization, including when you want to resynchronize the same repositories. |

The versions are independent; they do not need to be equal. ReaPackLab
checks central instructions on startup and periodically while REAPER runs.
Successful installations are retained even when another repository fails.
All approved repositories are attempted; a failed repository does not stop
attempts on later repositories. **The overall sync revision is marked
complete only after a successful run**, so failures remain eligible for retry.
Destructive repository cleanup is deferred while a sync has errors.

### Repository instructions

The `[Repositories]` section is one editable list of HTTPS index URLs:

```ini
[Repositories]
https://example.com/active/index.xml
xhttps://example.com/old/index.xml
-https://example.com/unwanted/index.xml
```

| Syntax | Meaning |
| --- | --- |
| Plain `https://…` | Subscribe and install/update packages. |
| `xhttps://…` | **Unsubscribe only.** Keep packages, dependencies, and installed files; do not touch other repositories. |
| `-https://…` | **Uninstall packages owned by that repository**, then unsubscribe. No dependency sweep or cross-repository uninstall. |

A URL cannot appear twice, including under different prefixes.
An ambiguous package owner prevents destructive cleanup. The built-in
ReaPackLabSetup repository cannot be removed by the hosted list.

**Safety note:** The new `x` and `-` behavior is implemented in the
development client, but repository-uninstall behavior still needs live
verification before classroom deployment. Do not trial `-` against a
repository holding essential extensions or scripts.

### Obsolete-package policy

This is a separate question from removing a repository. When an index no
longer lists an installed package, the central INI can specify:

```ini
[Options]
obsolete_packages=remove
```

`remove` (the **default**, even without the `[Options]` section) removes
obsolete, unpinned packages during a successful managed synchronization
without asking for a teacher password on each computer. Set
`obsolete_packages=keep` to leave them installed. Cleanup is skipped
when synchronization reports errors.

### ReaPackLabSetup

ReaPackLab has an internally configured source at
[ReaPackLabSetup](https://github.com/rockumk/ReaPackLabSetup). The client
attempts to synchronize that source even when it is not explicitly listed
in the hosted INI. **ReaPackLabSetup is not yet a finished installable
binary.** Its absence is expected during development and must not prevent
other repositories from being attempted.

## Access and security

The teacher password is represented by a salted PBKDF2-HMAC-SHA-256
verifier, not stored as plaintext in the INI. Because this repository is
public, the verifier is publicly readable and could be subjected to
offline password guessing; use a strong, unique password.

HTTPS authenticates transport but **the current development configuration
is not independently digitally signed**. Restrict write access to this
repository. Standard/student users should not be able to change their
central configuration or trigger their own managed package operations.

## Related repositories

- [ReaPackLab](https://github.com/rockumk/ReaPackLab) — public extension distribution
- [ReaPackLabDev](https://github.com/rockumk/ReaPackLabDev) — development and tests
- [ReaPackLabSetup](https://github.com/rockumk/ReaPackLabSetup) — planned installer/repair companion

## License

The configuration repository is MIT licensed (see [LICENSE](LICENSE)).
The ReaPackLab extension is separately subject to its LGPL licensing.
