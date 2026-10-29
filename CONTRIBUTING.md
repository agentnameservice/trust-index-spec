# Contributing

Follow the [ANS contribution guide](https://github.com/agentnameservice/governance/blob/main/CONTRIBUTING.md)
and [governance](https://github.com/agentnameservice/governance/blob/main/GOVERNANCE.md).
They define DCO sign-off, pull request review, issue references, and disclosure
of AI assistance.

The [Code of Conduct](CODE_OF_CONDUCT.md) applies to all participation.
Report vulnerabilities privately using [SECURITY.md](SECURITY.md).
The repository's [Maintainers](MAINTAINERS.md) handle reviews and releases.

## Specification changes

When changing normative requirements, update the affected JSON Schemas in
Appendix A or Appendix B of [TRUST_INDEX_SPEC.md](TRUST_INDEX_SPEC.md) and any
examples that depend on them in the same pull request.

## Spec and Schema Versions

`TRUST_INDEX_SPEC.md` carries two independent version numbers, stated in its
header: the spec version and the Trust Manifest schema version it defines.
Spec version 1.1.0 defines schema version 1.0.0, and non-breaking spec
revisions do not bump the schema version.

When proposing a change, say in the pull request which of the two you believe
it affects:

- **Editorial** (wording, examples, clarification that does not change a
  conformance requirement): neither version changes.
- **Spec revision** (a new SHOULD/MAY, a new signal, added prose): bump the
  spec version; leave the schema version alone.
- **Schema change** (a field added, renamed, or removed in Appendix A or
  Appendix B, or a changed conformance MUST): bump both, and note the
  migration path for existing manifests. Section 3.4 defines the
  current/deprecated/rejected lifecycle that governs how signal schema
  versions retire.
