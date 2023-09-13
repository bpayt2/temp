# Infrastructure-as-code Pipeline

The purpose of this document is to define the target state IaC CI/CD.

[Objectives](#objectives)

[Requirements](#requirements)

[Flow Diagram](#flow-diagram)


## Objectives

The design has the following objectives:

1. Deliver consistent infrastructure testing and instantiation
2. Provide an opinionated framework for infrastructure release
3. Be expandable to layer in application source code
4. Simplify IaC request and consumption


## Requirements

The design must achieve the following:

### Request

1. Provide a programmatic interface experience (e.g. git)

<details>
  <summary>Rationale</summary>
  - Integration with developer tools/IDEs enhances the experience.
  - Git maintains history, audit, and versioning.
</details>

2. Code abstraction (e.g. manifest driven)
3. Triggered actions/fulfillment (e.g. commit)

### Build

1. Build on curated templates (e.g. terraform modules)
2. Source configuration metadata (e.g. costcenter, support-team, etc.)
3. Evaluate rendered manifest against policy-as-code (i.e. cybersec standards)
4. Publish resultant manifest (e.g. tfvars)

### Test

1.
2. Record and notify test results
3.

### Promotion

1. Register request and approval via change management system
2. Promote manifest (e.g. pull-request to protected branch)
3. Increment release version

### Release

1. Validate change #
2. Deploy manifest and store state (if separated)
3. Notify and register metric (i.e. deployment frequency)

### Reconciliation
