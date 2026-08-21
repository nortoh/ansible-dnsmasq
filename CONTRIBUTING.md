# Contributing

Ansible role that installs and configures dnsmasq as a DNS forwarder and/or DHCP server on
CentOS/RHEL 7 and Fedora 16 or newer.

## Development setup

See [AGENTS.md](AGENTS.md#commands) for how to fetch the test suite and what CI runs — not
repeated here.

## Opening a PR

Use this repo's [pull request template](.github/PULL_REQUEST_TEMPLATE.md) — link the ticket or
issue and describe how the change was verified.

## Before you open a PR

- [ ] Ran the functional test suite locally, or explained why that wasn't possible
- [ ] `ansible-playbook --syntax-check` passes against the test playbook
- [ ] Docs (`README.md`/`AGENTS.md`) updated if this changes role variables, supported
      platforms, or how the role is tested
