# The Ærr Trinity

A Cost-of-Inquiry Law at Three Orders

| Field | Value |
|---|---|
| Author | Art Seabra · Ifthis Research |
| Date | 2026-05-27 |
| Version | v0.4 (preprint) |
| License | [CC BY 4.0](../../LICENSE) |
| DOI | 10.5281/zenodo.TBD |

## Abstract

The paper introduces the Ærr Trinity: a unified cost-of-inquiry framework formalized at three orders — the floor, the discipline, and the drift.

- **First order — the floor.** The Interrogation Cost Law (ICL), a law schema stating that a genuine answer to a demand must pay a cost bounded below by the structural complexity of that demand.
- **Second order — the discipline.** The Ærr Coefficient, κ, measuring how often a system pays the floor rather than accepting a surrogate.
- **Third order — the drift.** The Ærr Rate, measuring how fast surrogate outputs accumulate when the floor is not paid.

The shared object is the *surrogate stratum*: outputs that pay less than the floor, pass local inspection, and still fail to satisfy the demand. The Trinity is a dependency stack: ICL defines the floor; κ measures whether the floor is paid; the Ærr Rate measures how fast unpaid floors accumulate into drift.

## Files

| File | What it is |
|---|---|
| [the-aerr-trinity.md](the-aerr-trinity.md) | The paper (canonical markdown source) |
| [the-aerr-trinity.pdf](the-aerr-trinity.pdf) | PDF export |
| [study-guide.md](study-guide.md) | Pedagogical companion (quiz, essay prompts, glossary) |
| [slides/the-aerr-trinity-slides.pdf](slides/the-aerr-trinity-slides.pdf) | Presentation deck (PDF export of the Keynote source) |

## Operational toolkit

The math in this paper is operationalized as the [Ærr Sensor](https://github.com/artseabra/aerr-sensor) MCP server.

| Paper section | Sensor tool |
|---|---|
| §5 Floor (ICL) | `compute_floor` |
| §6 Discipline (κ) | `measure_kappa` |
| §7 Drift (Ærr Rate) | `aerr_rate` |
| §8 Chain Drift | `chain_drift` |
| §10 Empirical convergence | `convergence_watch` |

## Empirical instances

The paper grounds the Surrogate Existence Postulate in three constructive instances (§5.4). Each has its own folder under [../../empirical-instances/](../../empirical-instances/).
