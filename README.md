# Reproduce x402 V2 payment-policy checks

[![Public policy examples](https://github.com/Pazificateur69/settlens-x402-policy-examples/actions/workflows/policy-examples.yml/badge.svg)](https://github.com/Pazificateur69/settlens-x402-policy-examples/actions/workflows/policy-examples.yml)

A valid schema does not establish that a payment quote matches the terms you approved.

This example keeps the reviewer policy fixed and evaluates five synthetic challenges:

| Case | Expected decision |
| --- | --- |
| Approved example | `pay` |
| Price doubles | `do_not_pay` |
| Recipient changes | `do_not_pay` |
| Resource changes | `do_not_pay` |
| Recipient policy missing | `review` |

[Guide and 60-second silent video](https://settlens.com/blog/check-x402-payment-policy) · [Try the browser checker](https://settlens.com/tools/x402-checker#guided-demo)

Clone or download this repository, review the fixtures and runner, then run with Node.js 20.19 or newer:

```sh
node x402-policy-regression.cjs x402-policy-cases.json
```

No packages, account, wallet or private Settlens CLI are needed. The runner submits the supplied fixtures to the public `evaluate_payment_requirements` tool at `https://settlens.com/mcp`; it never requests the example merchant URL. Each run makes five requests, under a public limit of 60/minute per IP. Never include credentials, keys or confidential payloads.

The job exits nonzero on an unexpected decision or an unavailable evaluator. These synthetic fixtures test evaluator decisions. They do not test a deployed merchant: a release check needs a separately reviewed capture from an endpoint you are authorized to test. Do not automatically approve changed live terms.

`pay` means that an option meets the supplied policy and evaluated checks. It is not an instruction to transfer funds, a signed report, a signature or settlement verification, or a security guarantee. No defect or customer incident is claimed.

Built by Alessandro / Settlens. Feedback welcome: which payment-term regressions do your existing tests already catch, and what remains manual?

## Use it in CI

The included GitHub Actions workflow runs the five cases on pushes and pull requests with Node.js 24, read-only repository permissions and no secrets. Fork this repository to try it, or copy the two example files and workflow into your own project after review. The workflow's expected runtime is bounded by a three-minute job timeout.

A green job means these synthetic decisions matched their expected values. The public evaluator must be reachable; outages or shared-runner IP rate limits can fail the job. This repository contains only the public demonstration fixtures, runner and documentation. The Settlens engine is maintained separately in a private repository.
