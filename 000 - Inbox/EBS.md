ebs can only be in one az, so it cant be in az-1 and connect to a machine in az-2
think of ebs volumes as Network USB sticks.

can be attached/detached
have a provisioned capacity, and it can be increased over time.

there is delete on termination attribute which is by default on for ebs root vol and off for external ones.

