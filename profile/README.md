<p align="center"><img src="https://raw.githubusercontent.com/go-facter/brand/main/social/go-facter.png" alt="go-facter" width="640"></p>

<h1 align="center">go-facter</h1>
<p align="center"><strong>Puppet's Facter system-inventory engine in pure Go — no cgo.</strong></p>

<p align="center">
  🌐 <a href="https://go-facter.github.io">Website</a> ·
  📚 <a href="https://go-facter.github.io/docs/">Documentation</a>
</p>

<p align="center">
  <a href="https://go-facter.github.io/docs/"><img alt="Docs" src="https://img.shields.io/badge/docs-mkdocs--material-0F766E?style=flat-square"></a>
  <a href="https://github.com/go-facter/facter/blob/main/LICENSE"><img alt="License: BSD-3-Clause" src="https://img.shields.io/badge/license-BSD--3--Clause-blue?style=flat-square"></a>
  <img alt="Go 1.26.4+" src="https://img.shields.io/badge/go-1.26.4%2B-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img alt="Coverage 100%" src="https://img.shields.io/badge/coverage-100%25-1a7f37?style=flat-square">
</p>

---

go-facter is a **pure-Go (no cgo) reimplementation of [Facter](https://www.puppet.com/docs/puppet/latest/facter.html)**,
Puppet's system-inventory engine. It discovers structured facts about the host —
operating system, kernel, networking, processors, memory, filesystems,
virtualization, uptime, identity and more — and exposes them through a small,
stable API designed to be bound onto Ruby's `Facter` interface (`Facter.value`,
`Facter[]`, `Facter.add`, `Facter.to_hash`).

Fact **names and structure follow Facter's schema** — the aggregate facts
(`os`, `networking`, `processors`, `memory`, …) with the flat legacy aliases
(`operatingsystem`, `osfamily`, `ipaddress`, …) alongside — so it is a drop-in
for Puppet manifests that reference either shape.

> **This is the engine; the Ruby surface binds onto it.** `go-facter/facter`
> resolves facts in pure Go; the sibling `go-ruby-facter` will map Ruby's Facter
> API onto it so Puppet manifests and Ruby code under `rbgo` can resolve facts.

## Repositories

| Repo | What it is |
|------|------------|
| [**facter**](https://github.com/go-facter/facter) | the engine: resolver framework, all core fact groups, custom + external facts, and the `Value` / `ToHash` / `Add` / `LoadExternalFacts` API |
| [**docs**](https://github.com/go-facter/docs) | MkDocs Material documentation, versioned with [mike], served at [/docs/](https://go-facter.github.io/docs/) |
| [**go-facter.github.io**](https://github.com/go-facter/go-facter.github.io) | the Hugo landing page |
| [**brand**](https://github.com/go-facter/brand) | logos and brand assets |

## Principles

- **Pure Go, zero cgo.** Cross-compiles and embeds anywhere; a static binary by
  default. Only the Go standard library is imported.
- **An engine, not a CLI.** A stable Go API a Ruby binding maps directly onto
  `Facter.value` / `Facter.add` / `Facter.to_hash`; the `facter` CLI is a thin
  consumer of it.
- **Faithful to Facter's schema.** Aggregate structured facts plus the flat
  legacy aliases, so it drops into manifests referencing either shape.
- **Injectable OS seams.** Every file read, command and interface enumeration is
  a seam, so per-OS collectors and their error branches are covered
  deterministically — without root, on every OS and architecture.
- **100% test coverage** is the target, enforced as a CI gate.

## Status

**Engine complete.** Fact groups: `os` (incl. `os.macosx`), `kernel*`,
`networking` (interfaces + bindings + primary), `processors`, `memory` (+swap),
`mountpoints`, `filesystems`, `disks`, `virtual` / `is_virtual` (hypervisor and
container detection), `system_uptime`, `timezone`, `identity`, `path`,
`facterversion`, plus the flat legacy aliases. Custom facts (`Add` / `AddFunc`)
and external facts (`facts.d`: JSON / YAML / txt + executables). CLI `facter`
prints the full set or a single dotted-path query as JSON/YAML. 100% coverage
including error branches, `gofmt` + `go vet` clean, CI green on Linux, macOS and
Windows and across the six 64-bit Go targets (amd64, arm64, riscv64, loong64,
ppc64le, s390x).

BSD-3-Clause.

[mike]: https://github.com/jimporter/mike
