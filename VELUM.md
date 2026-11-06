# Velum's changes to sing-box

This is the sing-box that [Velum](https://github.com/Foreverwzh/velum) builds against. Branch `velum` carries a small set of
changes on top of an upstream release; each release is tagged `v<upstream>-velum.<n>`.

## Current changes

**Failed lookups do not fail the connection (`route/route.go`).** The `resolve` rule action exists so later IP rules can match.
When the lookup fails, those rules cannot match, but the connection may still reach a rule that does not need an IP. It now
keeps matching rather than being closed.

**Remote outbounds get the domain (`route/conn.go`).** After `resolve`, upstream dials the resolved addresses through whichever
outbound was selected. The local answer may be poisoned, or be a CDN address close to this device rather than to the node, so
domains are now handed to non-direct outbounds unresolved and the node resolves them. Direct outbounds still use the addresses.

## Updating to a new upstream release

Rebase this branch onto the new upstream tag, then tag it `v<upstream>-velum.1`. Update the `replace` line in Velum's `core/go.mod`.
