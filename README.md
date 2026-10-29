# Trust Index Open Specification

My company's payment agent is about to wire $50,000 to a supplier's invoicing
agent. It can already verify that the invoicing agent's identity is sealed in a
Transparency Log, that its domain ownership was validated, and that its
certificate fingerprint matches the TLSA record. None of that answers the
question it actually needs answered: should this agent be trusted to handle
$50,000?

A Trust Index answers it. It crawls Transparency Logs published by federated
Registration Authorities, combines the sealed data with external signals, and
computes a trust evaluation that says what category of tasks a client should
delegate to an agent.

## The specification

**[TRUST_INDEX_SPEC.md](TRUST_INDEX_SPEC.md)** — spec version 1.1.0, defining
Trust Manifest schema version 1.0.0.

An evaluation is a **Trust Vector**: five integers, each 0-100, scored
independently so that a strong score in one dimension cannot paper over a weak
one elsewhere.

| Dimension | The question it answers |
| --- | --- |
| Integrity | Is this agent mechanically sound? |
| Identity | Who stands behind this agent? |
| Solvency | Can this agent pay for damages? |
| Behavior | How does this agent treat others? |
| Safety | Will this agent leak data or cause harm? |

An agent scoring 95 on identity and 12 on solvency has strong accountability and
no financial backing. A news reader can use it; a payment agent should refuse the
wire. A single composite score hides exactly that distinction, which is why the
vector is the contract and `compositeScore` is deprecated.

Alongside the vector, an evaluation carries a recommended profile
(`READ_ONLY`, `TRANSACTIONAL`, `FIDUCIARY`, `UNTRUSTED`) for policy engines, and
a `riskFactors` array naming specific actionable concerns for human operators.
The whole payload is delivered as a signed W3C Verifiable Credential, so any
third party can verify an evaluation against the provider's public key without
crawling logs or recomputing scores.

## Document map

| Section | Contents |
| --- | --- |
| 1 | Purpose, terminology, conformance levels, relationship to the ANS architecture |
| 2 | Trust model: five signal categories, Trust Vector, profiles, signal decay, environment adjustments |
| 3 | Trust Manifest: required and optional fields, signal blocks, versioning lifecycle |
| 4 | Credential formats: X.509, W3C VCs, zero-knowledge proofs, SCITT receipts, hosted-platform delegation |
| 5 | Identity model: grades, attestation paths, principal binding, PriCC chains, inherited trust |
| 6 | Verification tiers: Bronze, Silver, Gold |
| 7 | Trust Evaluation API: request and response format, fresh vs cached, challenge-response |
| 8 | Federation: manifest portability, consortium governance, negative reputation sharing, dispute resolution |
| 9 | Security considerations: threat model, quantum resistance, liability |
| Appendix A | Trust Manifest JSON Schema (normative) |
| Appendix B | Trust Evaluation payload schema (normative) |
| Appendix C | Interaction context model (informational) |
| Appendix D | Signal decay model and recommended rates (informational) |
| Appendix E | Design rationale (informational) |

## What this specification does not define

It does not define signal weights, scoring algorithms, or deployment
architectures. Two conforming providers given the same Trust Manifest may return
different Trust Vectors, the same way two credit bureaus score one borrower
differently from identical financial records. The dimensions and their semantics
are fixed; the math inside each dimension is not.

A conforming Trust Index provider accepts Trust Manifests conforming to Appendix
A, returns evaluations as Verifiable Credentials conforming to Appendix B,
scores all five dimensions without collapsing them, supports the four
recommended profiles, verifies the credential formats in Section 4, and exposes
the Trust Evaluation API in Section 7. Section 1.3 states the requirements
normatively.

## Relationship to ANS

The Trust Index consumes the Agent Name Service rather than forming a layer of
it. ANS provides the sealed registration events, dual certificates, and DNS
records that a Trust Index scores; the protocol itself lives in
[agentnameservice/ans-spec](https://github.com/agentnameservice/ans-spec)
as the layered ANS-0 through ANS-6 specifications. Section 1.4 maps each ANS
concept to the dimension it feeds.

Because the conformance surface and release cadence are independent of the ANS
protocol specs, this document is versioned and published on its own.

[agentnameservice/agent-trust-discovery](https://github.com/agentnameservice/agent-trust-discovery)
is a Go reference implementation that indexes registered agents and serves
explainable five-dimension trust scores through a search API.

## Contributing

This repository follows [ANS governance](https://github.com/agentnameservice/governance/blob/main/GOVERNANCE.md).
See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidance and
[MAINTAINERS.md](MAINTAINERS.md) for the repository's Maintainers.
The [Code of Conduct](CODE_OF_CONDUCT.md) applies to participation;
report vulnerabilities privately using [SECURITY.md](SECURITY.md).

## License

Apache License 2.0. See [LICENSE](./LICENSE).
