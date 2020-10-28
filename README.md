# samutev: Salt Multipass Test Vm's

script to deploy quickly local test vm's using multipass.
test-vms can be deployed either as masterless minion or as a master with minions.

# requirements
- multipass
- a salt-repo-base-directory with salt-states and salt-pillars inside
- internet connection (for things like package install)

# configuration

in `samutev.conf` customize these settings to your need:
1. `salt_base=""`
2. `my_ssh_pub_key=""`

_First_ setting should be the directory of your salt project, where salt-states and salt-pillars reside.  
_Second_ setting is used to deploy the ssh-pub-key to each launchend vm to `root` and to the normal user `user`. So you do `ssh root@<vm-ip>` or `ssh user@<vm-ip>`

Further, in `samutev.conf` you can customize [cloudinit](https://cloudinit.readthedocs.io/en/latest/) to bootstrap the vms.

# usage
```
./samutev.sh -h
Usage:
    ./samutev.sh -h          # Display this help message.
    ./samutev.sh -n <VM>     # new <VM> with masterless minion
    ./samutev.sh -s <VM>     # new <VM> with minion and salt master. FIRST vm=saltmaster. minimum 2 vm's
    ./samutev.sh -d <VM>     # delete <VM>
    ./samutev.sh -l          # list   VM's

Examples:
    # --- masterless minions
    ./samutev.sh -n  testvm                     # launch new testvm              as masterless minion
    ./samutev.sh -n 'testvm1 testvm2 testvm3'   # launch multiple new testvm's   as masterless minions
    ./samutev.sh -n 'testvm1:c2:m1:d3 testvm2:c4:m2'
                                                # launch multiple new testvm's   as masterless minions
                                                #   with special settings for cpu, memory and disk
                                                #   testvm1 with 2 cpu, 1GB memory and 3GB disk
                                                #   defaults are c2 m1 d3
    
    # --- one salt-master with at least one minion
    ./samutev.sh -s 'salt-master1 testvm1 testvm2 testvm3'
                                                # launch a saltmaster with multiple new testvm's
                                                #   FIRST vm = saltmaster
                                                #   minimum = 2 vm's
    ./samutev.sh -s 'salt-master1:c2:m2:d6 testvm1'
    
    # --- delete test vm's
    ./samutev.sh -d  testvm                     # delaunch/delete testvm
    ./samutev.sh -d 'testvm1 testvm2 testvm3'   # delaunch/delete multiple testvm's
```