# ansible-dnsmasq

Ansible role that installs and configures dnsmasq as a DNS forwarder and/or DHCP server on
CentOS/RHEL 7 and Fedora 16+.

## Architecture in a paragraph

The role exists so dnsmasq setup is declarative and idempotent instead of hand-edited per host:
set role variables, and it installs the package, renders `/etc/dnsmasq.conf` from a single
Jinja2 template, and keeps the service running. `tasks/main.yml` is the only entry point — three
tasks run in sequence (install package, template the config, ensure the service is started and
enabled), and the template task validates the rendered config with `dnsmasq --test` before
dnsmasq ever reloads it. Every role variable is optional: `defaults/main.yml` sets four
security-related booleans, and everything else is conditionally emitted by
`templates/etc_dnsmasq.conf.j2` only when defined, so an empty variable set still produces a
working forwarder. The role declares no Galaxy dependencies (`meta/main.yml`) and deliberately
excludes firewall management — pair it with a distro-specific firewall role.

## File map

```
.travis.yml               # CI (Travis): Docker matrix per OS/version, syntax-check + apply +
                           # idempotence check + BATS tests — see Commands, requires the `tests` branch
CHANGELOG.md               # Keep a Changelog format, semantic versioning
defaults/
  main.yml                 # Defaults for the 4 security/behavior booleans; all other variables are unset by default
handlers/
  main.yml                 # "restart firewalld" handler actually restarts the dnsmasq service (not firewalld)
                           # and nothing notifies it — treat as dead code, not a working firewalld hook
LICENSE.md
meta/
  main.yml                 # Galaxy metadata; declares no role dependencies
README.md
tasks/
  main.yml                 # Install package -> template config -> ensure service running/enabled
templates/
  etc_dnsmasq.conf.j2       # Single template; every non-boolean setting is conditionally emitted only if defined
```

## Commands

No `.ansible-lint` or `.yamllint` config exists in this repo, so there's no local lint command
to run. The only test/CI definition is `.travis.yml`, and it depends on functional test code kept
in a separate `tests` git branch (see `.gitignore`, which excludes `tests/` from `master`).

This repo's `origin` does have a `tests` branch (confirmed via `git ls-remote`) — fetch it
from `origin`, not from the upstream [bertvv/ansible-dnsmasq](https://github.com/bertvv/ansible-dnsmasq),
which carries its own divergent `tests` branch and would exercise upstream's playbook
instead of this fork's:

```bash
git fetch origin tests
git worktree add tests origin/tests      # requires git >= 2.5.0
cd tests
vagrant up                               # builds the test VMs and applies test.yml
./runtests.sh                            # BATS functional tests; installs BATS on first run
```

What Travis runs against that same `test.yml`, inside a Docker container per matrix entry
(ubuntu 12.04/14.04, centos 6/7):

```bash
ansible-playbook tests/test.yml --syntax-check
ansible-playbook tests/test.yml
# idempotence: re-running the playbook should report changed=0 failed=0
ansible-playbook tests/test.yml | grep -q 'changed=0.*failed=0'
```

## Conventions

- Firewall configuration is out of scope by design. Don't add firewalld/iptables tasks here —
  use a separate distro-specific firewall role instead.
- `meta/main.yml` lists EL 7 and Fedora 16-23 as supported platforms, but `.travis.yml`'s test
  matrix also covers Ubuntu 12.04/14.04 and CentOS 6. The two have never been reconciled; treat
  the platform list in `meta/main.yml` as the supported set.

## See also

- [README.md](README.md) — role variables, example playbook, license, contributors
- [CHANGELOG.md](CHANGELOG.md) — version history
