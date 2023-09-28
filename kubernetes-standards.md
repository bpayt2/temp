# Kubernetes Standards

##### Department: IT Infrastructure and Architecture
##### Organization: Technology Strategy and Innovation
##### Domain: Data Center & Infrastructure Info Systems
##### Documentation Identifier: TBD
##### Template Version 1.0
##### Last Modified 9/27/2023
##### Author: Bryan Payton
##### Sponsor: Alex Dymanis and Tom Ciardo

# Summary

Kubernetes first entered Ulta Beauty in 2020 charged to deliver a flexible non-prescriptive application hosting platform that allows for rapid innovation, while at the same time providing consistent, common patterns for security, networking, and physical/virtual resources across cloud providers, unencumbered by legacy processes. Since then, Kubernetes has been solidified as Ulta Beauty's internal application platform of choice. The Kubernetes ecosystem is the collection of products and services that are built to best propel and empower Ulta Beauty’s present and future technology focus and optimization.

The standards set forth within this document serve to ensure the strategic alignment, optimal use and performance, availability and reliability, and overall Ulta Beauty success.

Standards are accessed against solutions to determine *well-architected* and production readiness.

[Standard Index](#standard-index)

[Standards](#standards)

[Responsibility Matrix](#responsibility-matrix)

[Appendix](#appendix)


## Standard Index

| Category | Standard | Number | Configuration Item Class|
| :-------- | :--------- | :------: | :-------- |
| Availability | Minimum replicas per deployment is 2 | [A1](#standard-1) | Application |
| Availability | Topology spread constraints are applied to each deployment  | [A2](#standard-2) | Application |
| Availability | Probes are applied to each deployment | [A3](#standard-3) | Application |
| Availability | Health checks are configured on application and checked by ingress | [A4](#standard-4) | Application |
| Availability | Anti-affinities are configured to spread workload across nodes | [A5](#standard-5) | Application |
| Availability | Pod distribution budgets are set for critical workloads | [A6](#standard-6) | Application |
| Resource Management | Quotas are applied to each custom namespace | [R1](#standard-1-1) | Infrastructure |
| Resource Management | Requests and limits are applied to deployments | [R2](#standard-2-1) | Application |
| Resource Management | Requests and limits are at-demand and equal for production workloads| [R3](#standard-3-1) | Application |
| Resource Management | Requests are lowered and limits are at-demand for non-production workloads | [R4](#standard-4-1) | Application |
| Resource Management | Node pools are create for different compute classes | [R5](#standard-5-1) | Infrastructure |
| Resource Management | Node overcommitted should be <=1.25 for production | [R6](#standard-6-1) | Application |
| Resource Management | Node selectors are applied to each custom namespace | [R7](#standard-7) | Infrastructure |
| Performance | Horizontal pod autoscalers are applied to all deployments | [P1](#standard-1-3) | Application |
| Performance | Vertical pod autoscalers are applied to monolithic and slow-start deployments | [P2](#standard-2-3) | Application |
| Performance | Ndots should be set to 1 | [P3](#standard-3-3) | Application |
| General | Namespaces are create per registered application (APM) | [G1](#standard-1-4) | Infrastructure |
| General | Init containers are used for startup processes | [G2](#standard-2-4) | Application |
| General | Configuration is externalized through configuration maps | [G3](#standard-3-4) | Application |


## Standards

### Availability

#### Standard #1

Minimum replicas per deployment is 2

##### Standard

Deployment configurations should define replicas as equal to 2, not 1.

<details>
  <summary>Example</summary>

  ```txt
    kind: Deployment
    metadata:
      name: example-deployment
      labels:
        app: example
    spec:
    **replicas: 2**
      selector:
        matchLabels:
          app: example
      template:
        metadata:
          labels:
            app: example
        spec:
          containers:
          - name: example-container
            image: ex-image:1.0.0
            ports:
            - containerPort: 80
  ```
</details>

##### Rationale

- Two replicas provides high availability for service load balancing
- Two replicas are used to position a workload across two fault domains (e.g. Google Cloud Platform zones or VMware hosts)
- More than two replicas should be initiated by scaling
- Removes single point of failure
- Creates smaller workload, thus faster scheduling and startup; load spread across two pods requires less upfront requests

##### Implications

- Requires accurate pod sizing to avoid cost increase

##### Caveats

- Stateful sets are not associated to this standard as default is generally 3

#### Standard #2

Topology spread constraints are applied to each deployment

##### Standard

Topology spread constraints should be used to ensure pod replicas are distributed evenly across fault domains.

<details>
  <summary>Example</summary>

  ```txt
    kind: Deployment
    metadata:
      name: example-deployment
      labels:
        app: example
    spec:
      # Configure a topology spread constraint
      topologySpreadConstraints:
        - maxSkew: <integer>
          minDomains: <integer> # optional; beta since v1.25
        **topologyKey: failure-domain.beta.kubernetes.io/zone**
        **whenUnsatisfiable: DoNotSchedule**
        **labelSelector: app = example**
          matchLabelKeys: <list> # optional; beta since v1.27
          nodeAffinityPolicy: [Honor|Ignore] # optional; beta since v1.26
          nodeTaintsPolicy: [Honor|Ignore] # optional; beta since v1.26
      ### other Pod fields go here
  ```
</details>

##### Rationale

- Clusters are provisioned to position nodes across fault domains (e.g. VMware hosts and GCP zones), but applications do not inherently benefit from this.
- Ensures scaling of replicas are distributed evenly across a desired number of fault domains
- Is more sophisticated then managing through a combination of affinities and anti-affinities (legacy approach)

##### Implications

- Affinities and anti-affinities need to be considered with topology spread constraints when applications need to collocated 'dependencies' on nodes

##### Caveats

- None

<!---
#### Standard #3

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #4

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #5

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #6

##### Standard

##### Rationale

##### Implications

##### Caveats

### Resource Management

#### Standard #1

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #2

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #3

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #4

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #5

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #6

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #7

##### Standard

##### Rationale

##### Implications

##### Caveats

### Performance

#### Standard #1

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #2

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #3

##### Standard

##### Rationale

##### Implications

##### Caveats

### General

#### Standard #1

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #2

##### Standard

##### Rationale

##### Implications

##### Caveats

#### Standard #3

##### Standard

##### Rationale

##### Implications

##### Caveats

--->

## Responsibility Matrix

| Number | Application Development | Infrastructure Engineering |
| :-------- | :--------- | :------: |
| [A1](#standard-1) | R, A, I | C |
| [A2](#standard-2) | R, A, I | C |
| [A3](#standard-3) | R, A, I | C |
| [A4](#standard-4) | R, A, I | C |
| [A5](#standard-5) | R, A, I | C |
| [A6](#standard-6) | R, A, I | C |
| [R1](#standard-1-1) | I | R, A, C |
| [R2](#standard-2-1) | R, A, I | C |
| [R3](#standard-3-1) | R, A, I | C |
| [R4](#standard-4-1) | R, A, I | C |
| [R5](#standard-5-1) | I | R, A, C |
| [R6](#standard-6-1) | R, A, I | C |
| [R7](#standard-7) | I | R, A, C |
| [P1](#standard-1-3) | R, A, I | C |
| [P2](#standard-2-3) | R, A, I | C |
| [P3](#standard-3-3) | R, A, I | C |
| [G1](#standard-1-4) | I | R, A, C |
| [G2](#standard-2-4) | R, A, I | C |
| [G3](#standard-3-4) | R, A, I | C |

**Legend**

R = Responsible

A = Accountable

C = Consulted

I = Informed
