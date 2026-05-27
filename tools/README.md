# Tools

The operational toolkit for the Ærr Frame lives in a separate repository:

→ [**Ærr Sensor**](https://github.com/artseabra/aerr-sensor) — MCP server measuring κ, Ærr Rate, and Chain Drift.

This folder is intentionally minimal; tools are versioned independently in `aerr-sensor`.

## Why a separate repo

The framework (this repo) and its measurement toolkit (Ærr Sensor) move at different velocities. Papers are stable artifacts; the toolkit is a living set of MCP tools that should be installable and updatable without bumping framework versions.
