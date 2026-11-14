# Disaster Recovery on AWS: A Practical Field Guide

A structured, practitioner-focused breakdown of AWS disaster recovery strategy, based on the AWS Well-Architected whitepaper "Disaster Recovery of Workloads on AWS: Recovery in the Cloud". Written for architects, technical programme leaders and engineering managers who need to choose, justify and test a DR strategy, not just read about one.

## Why this repo exists

Most teams conflate high availability with disaster recovery. They are different disciplines with different objectives. Availability measures mean values over time and protects individual components. Disaster recovery measures your response to one-time, large-scale events and protects discrete copies of the entire workload. A Multi-AZ deployment with auto scaling can be highly available and still have no answer to a corrupted database, a compromised account or a regional outage.

This repo distils the AWS guidance into decision-ready material: what each strategy costs, what it protects against, which services implement it, and the traps that invalidate a plan on paper.

## The four strategies at a glance

![DR strategy spectrum](assets/fig06-dr-strategies.png)

| Strategy | Typical RPO / RTO | Relative cost | DR Region state | Right for |
|---|---|---|---|---|
| Backup and restore | Hours | $ | Backups only | Lower priority workloads, data disaster protection for everything |
| Pilot light | Tens of minutes | $$ | Data live, compute off | Business critical workloads with moderate RTO |
| Warm standby | Minutes | $$$ | Scaled-down copy, always on | Business critical workloads needing fast, testable recovery |
| Multi-site active/active | Near real time | $$$$ | Full capacity, serving traffic | Mission critical, near-zero downtime requirements |

The first three are active/passive: one site serves traffic while another stands ready. Multi-site active/active serves production traffic from every Region, so there is no failover event at all.

## Repository guide

| Document | What it covers |
|---|---|
| [01 Fundamentals](docs/01-fundamentals.md) | What counts as a disaster, HA vs DR, shared responsibility, RTO and RPO, the cost trade |
| [02 Backup and Restore](docs/02-backup-and-restore.md) | Point-in-time backup services, AWS Backup, S3 CRR and versioning, automated restore testing |
| [03 Pilot Light](docs/03-pilot-light.md) | Continuous replication services, scaled-down IaC, failover routing, AWS Elastic Disaster Recovery |
| [04 Warm Standby](docs/04-warm-standby.md) | The pilot light distinction, Auto Scaling trade-offs, hot standby and static stability |
| [05 Multi-site Active/Active](docs/05-multi-site-active-active.md) | Write global, write local and write partitioned patterns, routing policies, StackSets |
| [06 Detection and Testing](docs/06-detection-and-testing.md) | Detection inside the RTO budget, deep health checks, drift management, Resilience Hub |
| [07 Choosing a Strategy](docs/07-choosing-a-strategy.md) | A decision checklist and the questions to ask in an architecture review |

## Five principles that outrank any diagram

1. **RTO and RPO are business decisions.** They come from business impact analysis, not from what the technology can do. If the recovery strategy costs more than the failure it prevents, do not build it unless regulation demands it.
2. **Replication is not backup.** Continuous replication faithfully delivers your data corruption to every copy. Point-in-time recovery is what protects you from destructive change.
3. **Fail over on the data plane.** Control plane operations have lower availability design goals and are exactly what may be impaired during a serious event. Route 53 health checks and Amazon Application Recovery Controller are data plane. Changing routing weights, Global Accelerator traffic dials and Auto Scaling API calls are not.
4. **Detection spends your RTO before recovery starts.** A one-hour RTO must contain detection, escalation, the decision to declare a disaster, and the recovery itself.
5. **The only recovery path that works is the one you test frequently.** Untested assumptions about capacity, quotas and AMI currency decay silently. Fewer recovery paths, exercised often, beat many paths exercised never.

<img width="882" height="574" alt="Screenshot 2026-11-14 at 04 53 35" src="https://github.com/user-attachments/assets/280bdfb7-d9dc-4bea-b1e7-c60ee33de017" />
<img width="949" height="456" alt="Screenshot 2026-11-14 at 06 25 20" src="https://github.com/user-attachments/assets/4ff164b4-36ac-4779-af4c-4e3a521bf48b" />
<img width="843" height="434" alt="Screenshot 2026-11-14 at 06 26 16" src="https://github.com/user-attachments/assets/cd28d084-f89c-45ee-a85e-646403ac08db" />
<img width="833" height="497" alt="Screenshot 2026-11-14 at 06 28 31" src="https://github.com/user-attachments/assets/05b51450-c8c3-4a85-9d3b-d8fc7a136fe1" />
<img width="827" height="494" alt="Screenshot 2026-11-14 at 06 28 43" src="https://github.com/user-attachments/assets/c6aa514f-b27d-4010-aeb0-ae7bb3eae97e" />
<img width="899" height="414" alt="Screenshot 2026-11-14 at 06 30 22" src="https://github.com/user-attachments/assets/2d88fc64-9834-4346-91dd-380948e80c0b" />
<img width="947" height="456" alt="Screenshot 2026-11-14 at 06 31 48" src="https://github.com/user-attachments/assets/9a19193d-5017-4c1a-80dd-652a4f86965a" />
<img width="1041" height="672" alt="Screenshot 2026-11-14 at 06 32 53" src="https://github.com/user-attachments/assets/9e639bbd-e977-45df-921c-f2b43148612d" />
<img width="861" height="468" alt="Screenshot 2026-11-14 at 06 33 53" src="https://github.com/user-attachments/assets/29d9e418-9e11-46af-9b0f-44716c259856" />
<img width="1021" height="505" alt="Screenshot 2026-11-14 at 06 34 49" src="https://github.com/user-attachments/assets/139a9c6f-1283-4ac4-ae29-5167b1eb3609" />
<img width="908" height="463" alt="Screenshot 2026-11-14 at 06 35 48" src="https://github.com/user-attachments/assets/040d893f-928b-48b1-a665-15195fba68c9" />
<img width="971" height="465" alt="Screenshot 2026-11-14 at 06 37 03" src="https://github.com/user-attachments/assets/ceb2c3f8-62d1-442a-8240-cffdcfb2454d" />


## Source and attribution

This material is a practitioner summary of the AWS whitepaper [Disaster Recovery of Workloads on AWS: Recovery in the Cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html), part of the AWS Well-Architected Framework. All architecture diagrams in `assets/` are from that whitepaper and are © Amazon Web Services, Inc. or its affiliates, included here for educational reference with attribution. See [assets/ATTRIBUTION.md](assets/ATTRIBUTION.md). The commentary and structure in the markdown files are my own.

## About the author

Anu Agarwal
* LinkedIn: [linkedin.com/in/agarwalanu](https://www.linkedin.com/in/agarwalanu)
* Substack: The Strategic Edge - https://anuagarwal.substack.com/p/your-cloud-architecture-is-highly 

If this guide helps you, a star helps others find it.
