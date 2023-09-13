# Infrastructure-as-code Pipeline

The purpose of this document is to define IaC CI/CD requirements to drive target state design.

[Objectives](#objectives)

[Requirements](#requirements)

[Flow Diagram](#flow-diagram)


## Objectives

The design has the following objectives:

1. Deliver consistent infrastructure testing and instantiation
2. Provide an opinionated framework for infrastructure release
3. Be expandable to layer in application source code and configuration management
4. Enhance infra-consumer and developer experience


## Requirements

The design must achieve the following:

### Request

1. Provide a programmatic interface experience (e.g. git)

    <details>
      <summary>Rationale</summary>

      - Integration with developer tools/IDEs enhances the experience
      - Promote flexibility for front end input (e.g. ServiceNow form, workflow orchestrators, hyper-automation tooling)
      - Git maintains history, audit, and versioning

    </details>

2. Code abstraction (e.g. manifest driven)

    <details>
      <summary>Rationale</summary>

      - Redundant code and data should be avoided
      - Enables extension into configuration management and application source code
      - Promotes flexibility for integrated pipeline technology (e.g. maven vs gradle, terraform vs ansible, etc.)
      - Promotes flexibility for testing frameworks and transitions (e.g. regression testing of source code but not infra-code)
      - JSON or YAML would be preferred

    </details>

3. Triggered actions/fulfillment (e.g. commit)

    <details>
      <summary>Rationale</summary>

      - Continuous
      - Fail fast
      - Aligns to developer user journey
      - Aligns to agile way of working

    </details>

### Build

1. Build on curated templates (e.g. terraform modules)

    <details>
      <summary>Rationale</summary>

      - Standardized and governed code
      - Reusable - variable driven
      - Centralizes change and 'vulnerability' management (e.g. infra type version X is no longer supported and needs to be Y)
      - NOTE - Should not be a blocker (i.e. backlog curated template creation in response to development deployment)

    </details>

2. Source configuration metadata (e.g. cost-center, support-team, etc.)

<details>
  <summary>Rationale</summary>

  - Enforces required metadata on all 'assets'/configuration items
  - Promotes application portfolio management and dependency accuracy
  - Promotes change management database accuracy
  - Enhances detection and response - MTTR (e.g. vulnerability on system X - OR - page team Y for system X alert)
  - Enables financial and operational observability

</details>

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
