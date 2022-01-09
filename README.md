## Scripts

#### General ubuntu update script

```
#!/bin/bash

APT_OPTIONS="-qq -o=Dpkg::Use-Pty=0 -y"

sudo echo "Starting upgrade of server + lxc hosts"

sudo apt $APT_OPTIONS update &> /dev/null
sudo apt $APT_OPTIONS upgrade
sudo apt $APT_OPTIONS autoremove &> /dev/null

for x in $(lxc list volatile.last_state.power=RUNNING -cn --format csv);
do
    echo Updating LXC host $x
    lxc exec $x -- apt $APT_OPTIONS update &> /dev/null
    lxc exec $x -- apt $APT_OPTIONS upgrade
    lxc exec $x -- apt $APT_OPTIONS autoremove &> /dev/null
done
````
