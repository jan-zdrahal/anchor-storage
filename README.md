# GitHub Anchors Storage

Document Date: 2026-07-15  
Version: 1.0  
Status: Active  

Author: Jan Zdráhal  
Website: https://www.zdrahal.eu  

Copyright © 2026 Jan Zdráhal

GitHub Anchors Storage is a public append-only storage repository preserving publicly observable Anchor Records.

The repository operates exclusively as the public storage continuity layer of the GitHub Anchors architecture.

---

## Repository Purpose

The repository preserves publicly observable Anchor Records for publicly projected continuity states.

Its purpose is to establish long-term append-only public storage continuity independent of artifact authority, governance, semantic interpretation, verification, or publication workflows.

---

## Repository Contents

The repository contains anonymous Storage Domains.

Each Storage Domain contains an append-only sequence of Anchor Records.

Repository history is append-only.

---

## Repository Layout

```text
domains/
  001/
    001.yaml
    002.yaml
  002/
    001.yaml
```

Each directory under `domains/` is an anonymous Storage Domain. Its name is a sequential identifier and exposes no semantic meaning.

Each file is a single immutable Anchor Record. Its name is a sequential index within the Storage Domain — the order in which anchors were added — independent of any external index or timestamp.

The sequential index is a storage-integrity ordering over the record set: the order in which anchors were added. It is not a state machine, a logical clock, or a temporal or causal ordering of external events. Event and execution semantics remain outside repository scope (see Repository Scope).

---

## Anchor Record

An Anchor Record is a minimal YAML file carrying a single anchored entry. It carries the entry and what is required to interpret it, and nothing more.

Every Anchor Record declares:

- `type` — the kind of anchor, which selects the procedure by which it is verified and the fields it carries.

All other fields depend on the `type`. The set of types is open and is not enumerated here. An anchored entry is not necessarily a hash: some anchor types anchor a hash, others anchor a record.

A hash-anchoring type carries `hash_algorithm` and `hash_value`. Example:

```yaml
type: snapshot
hash_algorithm: SHA3-256
hash_value: 29104d7c90617ca02eca20fe3c328fc1ede16b403666b1b411064683823972ba
```

A record-anchoring type carries the fields of the record it anchors. Example:

```yaml
type: record
space_id: 297a6bfe-13f5-462a-a102-d60e5bfe9fbe
step: 1
record_type: kernel_step
timestamp: "2026-06-08T05:07:56.233239Z"
```

---

## Repository Properties

GitHub Anchors Storage provides:

- append-only storage continuity
- sequential ordering of Anchor Records
- observable anchor ancestry
- long-term reconstruction compatibility

---

## Repository Scope

GitHub Anchors Storage does not define:

- artifact semantics
- event semantics
- execution semantics
- governance
- truth authority
- verification authority
- canonical artifact ownership
- synchronization semantics

These concerns are defined outside this repository.

---

## Repository Rules

The repository SHALL remain append-only.

Repository history SHALL NOT be rewritten.

Existing Anchor Records SHALL NOT be modified, renamed, or deleted.

---

## Conformance

An Anchor Record is admissible iff:

- it declares `type`;
- it carries the fields required by its `type`, and its entry is reproducible under the procedure selected by its `type`;
- it is immutable — once committed, never modified, renamed, or deleted.

A Storage Domain is admissible iff:

- its Anchor Records form a gap-free sequential index (`001`, `002`, …);
- index order is stable and append-only.

The gap-free sequential index is a storage-integrity condition over the record set — it detects a missing record — not a semantics of external events. It imposes no state machine, logical clock, or causal ordering (see Repository Layout).

Enforcement boundary. Git does not itself enforce append-only; repository history can be force-rewritten. Append-only is therefore a declared invariant whose guarantee is external to this repository — established by independent anchoring and by verification that detects any rewrite. This repository is a public projection surface, not an authority.

Writer. Anchor Records are written by a single writer. Sequential index allocation assumes no concurrent writers.
