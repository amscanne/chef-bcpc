VMS+Cobalt
==========

Setup
-----

1. Setup the `vms_key`.

 * If you haven't bootstrapped your environment yet, you can add the
   VMS key via `environments/Test-Laptop.json`. Under `bcpc` create a
   key `vms_key` and fill it in with your download key.

 * If you already have your environment, on the bootstrap node run:
   ```
   knife environment edit Test-Laptop
   ```
   And add `vms_key` under `bcpc` with your download key.

2. Re-run chef-client on all nodes.

Usage
-----

1. Install the cobaltclient extensions.

 * Via the apt repositories:
   ```
   # Install Gridcentric signing key
   wget -O - http://downloads.gridcentriclabs.com/packages/gridcentric.key | sudo apt-key add -
 
   # Add Gridcentric repositories
   echo deb http://downloads.gridcentriclabs.com/packages/cobaltclient/grizzly/ubuntu/ gridcentric multiverse | sudo tee -a /etc/apt/sources.list.d/cobaltclient.list
  
   # Update apt
   sudo apt-get update
   
   # Install the novaclient plugin for all users
   sudo apt-get install -y cobalt-novaclient
   ```

 * Or via pip:
   ```
   # Install the novaclient plugin for the current user
   pip install --user cobalt_python_novaclient_ext
   ```

2. Use it.

 Example usage:
 ```
 # Generate a keypair for instances.
 ssh-keygen
 nova keypair add ubuntu --pub_key=.ssh/id_rsa.pub

 # Boot a new instance the old-fashioned way.
 nova boot --key_name ubuntu --image <cirros> --flavor 1 --availability_zone=Test-Laptop <instance>

 # Install the agent in the instance (for IP reconfig, etc.).
 nova co-install-agent --user cirros <instance>

 # Create a new live image.
 nova live-image-create <instance> <live-image>

 # Launch a clone.
 nova live-image-start --live-image <live-image> <live-instance>
 ```
