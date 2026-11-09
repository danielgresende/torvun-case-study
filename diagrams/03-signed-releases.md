# Signed releases, verified before install

The desktop and mobile client can open a browser, a terminal and files. That makes an update a privileged operation, so every release is signed and a device installs only a release whose signature verifies.

```mermaid
sequenceDiagram
  autonumber
  participant B as Build and publish
  participant S as Release signer
  participant O as Owner
  participant P as Release server
  participant A as Desktop / mobile app
  B->>S: artifact and manifest
  S->>O: approve this exact signature
  O->>S: approval
  S-->>B: signed artifact and manifest
  B->>P: publish
  A->>P: check for a release
  P-->>A: signed release
  A->>A: verify the signature
  alt signature verifies
    A->>A: install the release
  else
    A->>A: refuse and keep the current version
  end
```

The signing key is not on the builder. A device verifies the artifact's signature before it runs it; a release that fails the check is refused and the current version stays. The owner approves the signature for one exact batch and the batch is published at most once an hour; that approval does not replace the check, so a release that does not verify is never installed.
