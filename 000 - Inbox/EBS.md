ebs can only be in one az, so it cant be in az-1 and connect to a machine in az-2
think of ebs volumes as Network USB sticks.

can be attached/detached
have a provisioned capacity, and it can be increased over time.

there is delete on termination attribute which is by default on for ebs root vol and off for external ones.

---
## Snapshots
Makes backup of an EBS instance,
No need to detach EBS from EC2, but recommended
Can copy across regions, and AZ


### Features
#### EBS snapshot archive
1. moves the snapshot to the archive tier which makes things cheaper 75%
2. it takes from 24 to 72 hrs to restore it
#### EBS Recycle bin
setup rules to retain deleted snapshots to recover after accidental deletion, the period is from 1 day to a year.

#### Fast Snapshot Restore
Force full init of the snapshot so it has no latency on the first use. (costs more)
