# Grafana Ansible Quick Start

Quickly provision a Vagrant box with:

* Graphite (http://graphite.wikidot.com/)
* Carbon (http://graphite.wikidot.com/carbon)
* Grafana (http://grafana.org/)

The Ansible playbooks and associated configuration have been adapted from https://github.com/pellepelster/graphite-grafana-vagrant-box

## Installation

```
git clone https://github.com/rosswilson/graphite-grafana-vagrant-box.git
cd graphite-grafana-vagrant-box
vagrant up
```

If you make any changes to the config files, run the Ansible provisioner again to push your changes to the VM:

```
vagrant provision
```

## Port Overview

* Graphite: http://localhost:1080/
* Carbon: http://localhost:2003
* Grafana: http://localhost:1080/grafana/
