# Terminology

> Proposal for improvements submitted to the
> [Sunbird RC project](https://github.com/sunbird-rc/sunbird-rc-core).

## Registry

Any data store that stores and provides API endpoints to create, access, modify
and delete data in accordance with this specification.

## Entity

Any record stored in the [registry](#registry).

## Schema

A minimum representation of all [entities](#entity) of the same type.

## Claim

An assertion made about an [entity](#entity), which must be
[verified](#attestation) to be considered true.

## Attestation

The process of evaluating a [claim](#claim) made about an [entity](#entity),
checking if it is indeed true, and then generating a cryptographically secure
[proof](#proof) to declare it so.

## Attestor

A role an [entity](#entity) might perform by receiving a representation of an
[entity](#entity) and putting its [claims](#claim) through the process of
[attestation](#attestation), if it is designated as attestor for a certain
[claim](#claim) made by another [entity](#entity).
