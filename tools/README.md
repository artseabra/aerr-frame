# Tools

Reviewed: 2026-07-08

The operational toolkit for the ÆrrFrame lives in a separate private repository:

→ **ÆrrSensor** — internal MCP server measuring κ, ÆrrRate, and Chain Drift.

This folder is intentionally minimal; tools are versioned independently in the private `aerr-sensor` repository.

## Why a separate repo

The framework (this repo) and its measurement toolkit (ÆrrSensor) move at different velocities. Papers are stable artifacts; the toolkit is a living set of MCP tools that should be installable and updatable without bumping framework versions.
