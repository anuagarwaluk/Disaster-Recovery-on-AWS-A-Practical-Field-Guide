# Choosing a Strategy: A Decision Checklist

A condensed sequence for selecting and defending a DR strategy, drawn from the whitepaper's logic.

## Step 1: Classify the disaster scenarios

For each workload, list the disasters that could plausibly hit it across the three categories (natural, technical, human) and their blast radius (local through global). Note which are already absorbed by a well-architected Multi-AZ deployment, most local events are, and which require multi-Region response: regional events, and data disasters that replication would propagate.

## Step 2: Set RTO and RPO from the business, not the stack

Run the business impact analysis. Who is affected, what does an hour of downtime cost, how does impact vary with timing. Combine with the risk assessment and the cost of candidate strategies. Legitimate outcomes include "no DR for this workload" as an informed decision. Do not build a recovery strategy that costs more than the failure it prevents, unless regulation requires it.

## Step 3: Map objectives to a strategy

| If your objectives are... | Start with |
|---|---|
| RTO/RPO in hours, cost minimised | Backup and restore |
| RTO/RPO in tens of minutes | Pilot light |
| RTO/RPO in minutes | Warm standby |
| Near-zero RTO, mission critical | Multi-site active/active (or hot standby if the second Region must not take traffic) |

Whatever the tier, point-in-time backup is non-negotiable, because every tier above backup and restore relies on replication, and replication propagates data disasters.

## Step 4: Audit the failover path for control plane dependencies

Walk the failover sequence call by call and classify each one.

* Data plane, keep: Route 53 health check evaluation and DNS failover, Amazon Application Recovery Controller switches
* Control plane, minimise or accept knowingly: Route 53 routing policy changes, Global Accelerator traffic dials, Auto Scaling calls, restore-from-backup operations

Where a control plane dependency remains, decide explicitly whether to pay for static stability (pre-provisioned capacity) instead.

## Step 5: Decide who pulls the trigger

Automatic failover from deep, multi-signal health checks for the most stringent RTOs, with false-alarm safeguards. Otherwise manual initiation with everything after the decision automated to a single action.

## Step 6: Schedule the tests before declaring the plan done

* Regular failover tests to the DR Region validating RTO and RPO
* Scheduled automated restore of backups
* Drift monitoring with AWS Config, CloudFormation drift detection, and quota checks in the DR Region
* Continuous validation with AWS Resilience Hub

## Five questions for the architecture review

1. What are the RTO and RPO, and who in the business signed them off?
2. Which strategy tier are we on, and does its cost match the workload's criticality?
3. Does the failover path depend on any control plane operation?
4. What protects us from a data disaster, not just a site disaster?
5. When did we last execute failover for real, and what broke?

---
Source: AWS, [Disaster Recovery of Workloads on AWS: Recovery in the Cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html).
