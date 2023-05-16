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
Next steps would be to disable [Pi-hole](https://pi-hole.net/) web interface and switch from [Tailscale](https://tailscale.com/) to bare [Wireguard](https://www.wireguard.com/).
 
<img width="1409" alt="image" src="https://github.com/RAV64/Rasp-Cleaner/assets/73443709/72a6ee2c-5d13-409d-8550-ea4602c9a925">

### `systemd-journal` using the most memory seems quite strange.

After tracking down what might be the cause of this I found out tailscale is flooding my journalctl. I utilized this chatGPT generated command to demonstrate it:

`journalctl | awk 'BEGIN{count=0} {if($0 ~ /tailscale/) count++} END{print "Total Lines: " NR; printf "Matched Lines: %d\n", count; printf "Percentage: %.2f%%\n", (count/NR)*100}'`

This was the output:

```
Total Lines: 59777
Matched Lines: 35931
Percentage: 60.11%
```

Options to fix this are changing to Wireguard or changing logging rules for Tailscale. [Here](https://github.com/tailscale/tailscale/issues/1548#issuecomment-1031152941) is a "fix" to disable Tailscale flooding journal logs.

This didn't solve the memory usage issue. 

Modifying `/etc/systemd/journald.conf` by adding `SystemMaxUse=50M` line after `[journal]` I have now succesfully got it off the top of my memory consumption list.
This has its own flaws but fits my use case. Also this new smaller size doesn't get flooded by Tailscale anymore so it should be enough.

This change is now automatically implemented in [this commit](https://github.com/RAV64/Rasp-Cleaner/commit/b4ef0675dbc65dbdf5564715d5a6649363e5d5fc)

## Sources

This was built for my own use and as an course exercise for [course](https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/)
