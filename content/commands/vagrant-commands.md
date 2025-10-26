```bash
vagrant init
vagrant up --provider=virutalbox

# Sleep: temporarily pause your work
vagrant suspend
vagrant resume

# Power: switch off the VM
vagrant halt
vagrant up

# Restart
vagrant reload

# Remove the VM
vagrant destroy

# Remove vagrant box
vagrant box remove hashicorp-education/ubuntu-24-04 # box name