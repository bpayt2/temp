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

**Summary Table**

| Requirement | Description | Phase | Release |
| ----------- | ----------- | ----- | ------- |
| [1](#requirement-1) | Provide a programmatic interface experience | CI - Request | MVP |
| [2](#requirement-2) | Abstract code | CI - Request | MVP |
| [3](#requirement-3) | Trigger actions/fulfillment  | CI - Request | MVP |
| [4](#requirement-4) | Build on curated templates | CI - Build | Full Product |
| [5](#requirement-5) | Source configuration metadata | CI - Build | Full Product |
| [6](#requirement-6) | Evaluate build against policy-as-code | CI - Build | Full Product |
| [7](#requirement-7) | Publish resultant build (manifest) | CI - Build | MVP |
| [8](#requirement-8) | Record and notify test results | CI - Test | Full Product |
| [9](#requirement-9) | Register request and approval via change management system | CI - Promotion | MVP |
| [10](#requirement-10) | Promote manifest | CI - Promotion | MVP |
| [11](#requirement-11) | Increment release version | CI - Promotion | MVP |
| [12](#requirement-12) | Validate change number | CD - Release | MVP |
| [13](#requirement-13) | Deploy build and store state | CD - Release | MVP |
| [14](#requirement-14) | Notify and register metric | CD - Release | Full Product |
| [15](#requirement-15) | Desired and current state are reconciled | CI - Reconciliation | Full Product |

### CI - Request

#### Requirement 1

*Provide a programmatic interface experience (e.g. git)*

<details>
  <summary>Rationale</summary>

  - Integration with developer tools/IDEs enhances the experience
  - Promote flexibility for front end input (e.g. ServiceNow form, workflow orchestrators, hyper-automation tooling)
  - Git maintains history, audit, and versioning

</details>

#### Requirement 2

*Abstract code (e.g. manifest driven)*

<details>
  <summary>Rationale</summary>

  - Redundant code and data should be avoided
  - Enables extension into configuration management and application source code
  - Promotes flexibility for integrated pipeline technology (e.g. maven vs gradle, terraform vs ansible, etc.)
  - Promotes flexibility for testing frameworks and transitions (e.g. regression testing of source code but not infra-code)
  - JSON or YAML would be preferred

</details>

#### Requirement 3

*Trigger actions/fulfillment (e.g. commit)*

<details>
  <summary>Rationale</summary>

  - Continuous
  - Fail fast
  - Aligns to developer user journey
  - Aligns to agile way of working

</details>

### CI - Build

#### Requirement 4

*Build on curated templates (e.g. terraform modules)*

<details>
  <summary>Rationale</summary>

  - Standardized and governed code
  - Reusable - variable driven
  - Centralizes change and 'vulnerability' management (e.g. infra type version X is no longer supported and needs to be Y)
  - NOTE - Should not be a blocker (i.e. backlog curated template creation in response to development deployment)

</details>

#### Requirement 5

*Source configuration metadata*

<details>
  <summary>Rationale</summary>

  - Enforces required metadata on all 'assets'/configuration items (e.g. cost-center, support-team, etc.)
  - Promotes application portfolio management and dependency accuracy
  - Promotes change management database accuracy
  - Enhances detection and response - MTTR (e.g. vulnerability on system X - OR - page team Y for system X alert)
  - Enables financial and operational observability

</details>

#### Requirement 6

*Evaluate build against policy-as-code*

<details>
  <summary>Rationale</summary>

  - Enforces cybersecurity rules via policy
  - Minimizes technical risk
  - Adds control point for best practice and standard alignment

</details>

#### Requirement 7

*Publish resultant build (manifest)*

<details>
  <summary>Rationale</summary>

  - Maintains code history and inventory
  - Enables disaster recovery (recover from deletion)
  - Supports bulk update

</details>

### CI - Test

#### Requirement 8

*Record and notify test results*

<details>
  <summary>Rationale</summary>

  - Historical code coverage and testing reports
  - Data point for change approval
  - Data point for code quality and health

</details>


### CI - Promotion

#### Requirement 9

*Register request and approval via change management system*

<details>
  <summary>Rationale</summary>

  - Audit record of change (Applies to Normal, Standard, Emergency and Exception changes)
  - Ensures Change Owner approval and adheres to change advisory board policy
  - Data point for deployment frequency

</details>

#### Requirement 10

*Promote code*

<details>
  <summary>Rationale</summary>

  - Moves code into protected branch
  - Standard code release and audit process
  - Trigger point for deployment

</details>

#### Requirement 11

*Increment release version*

<details>
  <summary>Rationale</summary>

  - Identifies source code version, used for correlating to deployed asset/configuration item
  - Change management identifier
  - Data point for deployment frequency and code sensitivity

</details>

### CD - Release

#### Requirement 12

*Validate change number*

<details>
  <summary>Rationale</summary>

  - Enforces adherence to change management process

</details>

#### Requirement 13

*Deploy build and store state*

<details>
  <summary>Rationale</summary>

  - End result
  - State required for reconciliation

</details>

#### Requirement 14

*Notify and register metric*

<details>
  <summary>Rationale</summary>

  - Closes change and notifies change owner
  - Records successful deployment, used for DORA (DevOps Research and Assessment team) metrics

</details>

### CD - Reconciliation

#### Requirement 15

*Desired and current state are reconciled*

<details>
  <summary>Rationale</summary>

  - Identifies drift from desired and current state
  - Enforces change via source code (this CI/CD process)
  - Discourages/reverts manual change

</details>
