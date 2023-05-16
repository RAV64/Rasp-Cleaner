# Rasp-Cleaner

an Ansible playbook to quickly remove excess packages and services which are not needed for
eth facing home server functionality.

## What is removed?
- Wlan
- Bluetooth
- Printers
- Desktop environment
- Games
- Audio

## Installation

### Ansible and its dependencies

On mac these can be installed with [Homebrew](https://brew.sh/) `brew install ansible`
Or it can be installed with pip `pip install ansible`

## Usage

Ensure you can form a ssh connection to target machine

Clone this repository and navigate to it

To run the whole playbook:
- `ansible-playbook -i <ADDRESS>, -u <USER> playbook.yml`

To run parts of the playbook:
- `ansible-playbook -i <ADDRESS>, -u <USER> playbook.yml --tags "<comma,separated,list,of,tags>"`

Valid tags are:
- printer
- wifi
- bluetooth
- desktop
- email
- sound
- webcam
- games
- auto

## Disclaimer

Playbook is a work in progress alpha product and will propably break something when ran except in certain use cases

## After thoughts 

Running this playbook leaves the "victim" machine in quite nice state if one does not need the services we're removing here.
Running `ps aux --sort=-%mem | head -30` to list top 30 most memory consuming processes gives a list that needs no actual cleaning.
Next steps would be to disable [pi-hole](https://pi-hole.net/) web interface and switch from [tailscale](https://tailscale.com/) to bare [wireguard](https://www.wireguard.com/).
 
<img width="1409" alt="image" src="https://github.com/RAV64/Rasp-Cleaner/assets/73443709/72a6ee2c-5d13-409d-8550-ea4602c9a925">

## Sources

This was built for my own use and as an course exercise for [course](https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/)
