---
processors : ["Neoverse-N1"]
software : ["linux"]
title: "Run Clair in the Distributed Mode"
type: docs
weight: 4
hide_summary: true
description: >
    Learn how to setup the configuration to run Clair in the Distributed Mode.
---

## Pre-requisites

* An Amazon Web Services(AWS) account.
* docker and docker-compose (latest version preferred).
* go (latest version preferred).

## Install and run Clair in the Distributed Mode

Using your AWS Account, launch an ARM 64-bit instance running Ubuntu.

Then follow [this documentation](https://quay.github.io/clair/howto/deployment.html#distributed-deployment) to run Clair in the distributed mode.

### Steps in brief

NOTE: The below mentioned steps are tested successfully with Clair v4.4.4.

In a distributed deployment, each Clair process i.e. indexer, matcher and notifier, runs in its own OS process. docker-compose.yaml already has targets defined to run these 3 modes. Unlike the Combo mode, all 3 modes will run inside containers. So, there is no need to expose postgres port 5432, as all 3 modes of Clair are in same container network as postgres is.

* Execute below command to setup postgres database:

```console
sudo docker-compose up -d clair-database
```

* We will need a load balancer to divert traffic to the correct service i.e. indexer, matcher and notifier as requested. We will use "Traefik" here, which will run on port 6060. To setup traefik, execute the below command:

```console
sudo docker-compose up -d traefik
```

* Next, run indexer, matcher and notifier as 3 separate processes. docker-compose.yaml already has targets defined for the same.

```console
sudo docker-compose up -d indexer matcher notifier
```

* You can verify if all 5 containers, i.e. clair-database, traefik, indexer, matcher, notifier, are running, using docker CLI:

```console
sudo docker ps
```

* To check logs in each of the Clair's service:

```console
sudo docker logs clair-indexer
sudo docker logs clair-matcher
sudo docker logs clair-notifier
```

* Now that everything is running, you can submit manifest for generating the vulnerability report.

[<-- Return to Learning Path](/content/en/cloud/clair/#sections)

[How to Generate the Vulnerability Report -->](/content/en/cloud/clair/vulnerability_report.md)
