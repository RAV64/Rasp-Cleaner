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

Playbook is a work in progress alpha product and will propably break something when ran except in certain usecases

## Sources

This was built for my own use and as an course exercise for [course](https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/)
