# Mother installer

Public distribution of the Mother installer and its bootstrap verifier.
Application packages remain private and require repository-scoped read access.

Each published installation command selects an immutable revision and verifies
its checksum before execution.

Never put access tokens, signing keys, or installation secrets in this repository.

## Pre-alpha installation candidate 2

This installer selects signed Manager and Workspace packages from the private
`kareemk/mother` repository for Ubuntu 24.04 on x86-64. The script and its
configuration are public; package downloads require your separate repository-only
Contents: read token with an expiration date. Never put the token in a command or
message. The installer asks for it privately in the terminal.

This candidate still needs native installation and the iPhone update/recovery
walkthrough. Use it only for the explicitly approved pre-alpha host. It is configured to provision
the dedicated `kiva` service account, with 2 GiB shared Agent memory and two
concurrent Agent runs. Existing installations require explicit owner-managed
retirement before fresh setup; startup does not silently reset data.

Installing creates a dedicated system user/group and rootful Podman/runsc Agent
engine with systemd services and a network-policy timer. It creates the
`mother-agent0` bridge (`172.31.240.0/24`), enables IP forwarding and bridge
netfilter, installs Agent firewall/NAT rules, and binds Workspace and Manager
listeners on `0.0.0.0:39082` and `0.0.0.0:39083`.

Download the complete installer, verify its digest, then execute it:

```sh
( f=$(mktemp) && trap 'rm -f -- "$f"' EXIT && curl --proto '=https' --tlsv1.2 --fail --silent --show-error https://raw.githubusercontent.com/kareemk/mother-install/installer-80eed893ca55c9aac704b687eb43b895e9729902-31d8bc7968fe2662a4960da79e1abec5fb9ce79238b637b80b0b8fcd6910ae74/installers/80eed893ca55c9aac704b687eb43b895e9729902/31d8bc7968fe2662a4960da79e1abec5fb9ce79238b637b80b0b8fcd6910ae74/install.sh -o "$f" && printf '%s  %s\n' 9ba6b4fc14d2223d98c19f7afe4ebb7132ade89458535442389686db5936274d "$f" | sha256sum --check --status && bash "$f" )
```

The Workspace owner approves Workspace updates in Mother. Manager maintenance is
performed on the server. Publishing a release does not install it on your host.
