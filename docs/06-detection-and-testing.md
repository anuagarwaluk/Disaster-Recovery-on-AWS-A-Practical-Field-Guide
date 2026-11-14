# Detection and Testing

The two chapters of the whitepaper most often skipped, and the two that decide whether any of the previous strategies work.

## Detection: the RTO you already spent

An RTO is not a recovery budget, it is a total budget. A one-hour RTO must contain:

1. Detecting the incident
2. Notifying the right people
3. Running escalation
4. Evaluating expected time to recovery without invoking DR
5. Declaring a disaster
6. The recovery itself

Teams that mentally allocate the full hour to step six have missed their objective before starting. Factor detection, notification, escalation, discovery and declaration into planning, or the objectives are not realistic.

**Sources of signal:**

* **AWS Service Health Dashboard**: up-to-the-minute AWS service availability, with RSS feeds per service
* **AWS Health Dashboard**: events affecting your specific account, as a categorised dashboard plus a 90-day event log
* **Your own health checks**: for the most stringent RTOs, automate failover on health checks that represent real user experience, built on key performance indicators. Use deep health checks that exercise key workload functionality, not shallow heartbeats, and base decisions on multiple signals. Be cautious: false alarms trigger failovers that themselves introduce availability risk and data loss.

**The organisational tell.** If stakeholders decline to invoke DR even as the RTO is at risk, the plans are inadequate or nobody trusts their execution. Either way, re-evaluate the plans and objectives. A plan nobody will pull the trigger on is not a plan.

## Testing: exercised paths are the only paths

The whitepaper's position, from AWS operational experience: "the only error recovery that works is the path you test frequently."

The anti-pattern is the rarely executed recovery path. A secondary data store that serves read-only queries looks like a ready failover target. Fail over to it untested and you discover the capacity that sufficed at last year's test no longer tolerates today's load, or that service quotas in the secondary Region are too low. Assumptions about recovery decay silently, and testing is the only instrument that detects the decay.

Consequences for design:

* **Keep the number of recovery paths small.** Every path needs regular exercise, so fewer is better.
* **Test failover to the DR Region regularly**, validating RTO and RPO are actually met.
* **Complex or critical recovery paths need regular execution in production.**

## Managing configuration drift

The DR Region must hold the infrastructure, data and configuration the plan assumes, including current AMIs and sufficient service quotas. Drift controls:

* **AWS Config** continuously monitors and records resource configurations, detects drift, and can trigger AWS Systems Manager Automation to remediate and raise alarms
* **AWS CloudFormation drift detection** catches divergence in deployed stacks
* **AWS Resilience Hub** continuously validates and tracks workload resilience, including whether RTO and RPO targets are likely to be met

---
Source: AWS, [Disaster Recovery of Workloads on AWS: Recovery in the Cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html).
