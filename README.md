# Grafana Ansible Quick Start

Quickly provision a Vagrant box with:

* Graphite (http://graphite.wikidot.com/)
* Carbon (http://graphite.wikidot.com/carbon)
* Grafana (http://grafana.org/)

The Ansible playbooks and associated configuration have been adapted from https://github.com/pellepelster/graphite-grafana-vagrant-box

## Installation

```shell
git clone https://github.com/rosswilson/graphite-grafana-vagrant-box.git
cd graphite-grafana-vagrant-box
vagrant up
```

This process can take a few minutes. Once finished, you'll have a shiney new virtual machine running Graphite, Grafana, and Jenkins.
* Grafana [localhost:1080/grafana](http://localhost:1080/grafana)
* Graphite [localhost:1080](http://localhost:1080/)
* Jenkins: [localhost:1080/jenkins](http://localhost:1080/jenkins)

> You'll find a Grafana dashboard called [Graphite Health](http://localhost:1080/grafana/dashboard/db/graphite-health). This is an example of accessing data from Graphite.

Follow the next section to setup Jenkins. It'll take you 2 minutes.

## Setup

### Jenkins User

The admin password for Jenkins is auto generated and stored in a file on the server. We need to read this file to proceed with the setup.

1. Run the following command in your Terminal window:

  ```shell
  vagrant ssh -- "sudo cat /var/lib/jenkins/secrets/initialAdminPassword"
  ```

1. Copy the 32 character password.
> Ignore the warning about "sudo: unable to resolve host ubuntu-xenial".

1. Visit your instance of Jenkins at [localhost:1080/jenkins](http://localhost:1080/jenkins) and paste in the copied password.

1. Click "Install suggested plugins" and wait for them to install.

1. Create an admin user - fill out the fields and pick a secure password.

### Jenkins GitHub Credentials

1. Run the following command in your Terminal window:

  ```shell
  vagrant ssh -- "sudo cat /var/lib/jenkins/.ssh/id_rsa.pub"
  ```

1. Copy the line starting "ssh-rsa". It wraps onto multiple lines, be careful.

1. Go to [GitHub Deploy Keys](https://github.com/bbc/comscore-graphite-ingest/settings/keys) for the comscore-graphite-ingest repo and register the SSH key you just copied.

### Jenkins DAX Credentials

1. Go to the Configure System section of [your instance of Jenkins](http://localhost:1080/jenkins/configure)

1. Tick the "Environment variables" option, it's under the Global properties heading.

1. Add two environment variables (names below) and set their value to the DAX API account username and password.

  * DAX_USER
  * DAX_PASSWORD

### Creating a Jenkins Job

Now, lets say you're using the [comscore-graphite-ingest](https://github.com/bbc/comscore-graphite-ingest) project and have created a new job file (a `.js` file in the `jobs` directory). I recommend creating a separate Jenkins job for each ingester job file. This way you have maximum control over the scheduling of your ingest operation job, and it keeps the console output in one place.

1. From the Jenkins home screen, click "New Item".

1. Give your job a decent name, choose the "Freestyle project" type, click Ok.

1. Under "Source Code Management", choose the Git radio button.

1. Set the Repository URL to: `git@github.com:bbc/comscore-graphite-ingest.git`

1. Choose the `Read-only comscore-graphite-ingest access` option from the Credentials drop down menu.

1. Click "Add build step" and then "Execute shell".

1. In the Command box, paste:

  ```sh
npm install
node ./src/ingester.js my_ingest_job_name_goes_here
  ```

Now run your job and check the console output. If you encounter errors, try hopping over to the [comscore-graphite-ingest](https://github.com/bbc/comscore-graphite-ingest) project for data-ingest specific help.

## Provision Again

If you make any changes to the config files, run the Ansible provisioner again:

```
vagrant provision
```

## Port Overview

* Graphite: http://localhost:1080/
* Carbon: http://localhost:2003
* Grafana: http://localhost:1080/grafana/
