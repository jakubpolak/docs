---
title: "Cloud Pricing"
navTitle: "Pricing"
description: "An overview of Astronomer Cloud pricing."
---

To view a general overview of our pricing structure across our product offerings, check out the [pricing page](/pricing/) on our website.

### Overview

As part of our promise to give our customers more freedom and control with Apache Airflow, Astronomer Cloud is priced based on exact resource usage per deployment. Your monthly charge is based on the total number of Airflow deployments tied to your organization and the total AU hours you allocate to each of those instances.

Our Cloud platform is fully managed on Astronomer's infrastructure, allowing you to run as many deployments (and DAGs) as you'd like. Each of your deployments is powered by an isolated set of resources that can be scaled up or down to fit your needs and the architecture of your running workflows.

Resource configuration is directly managed on the `Configure` tab of the Astronomer UI (check out our [Astronomer UI Guide](/docs/airflow-deployments/) for more detail).

### Configuration Overview

For us to maximize your control over your Airflow deployments, our monthly cost to run Astronomer Cloud fully depends on how much you scale each component of your Airflow instance(s) - and for how long - throughout the course of your billing cycle.

Via the [Astronomer UI](https://app.gcp0001.us-east4.astronomer.io/login) as seen below, you can freely scale the following components of each of your instances:

- Webserver
- Scheduler
- Celery Worker Count
- Celery Worker Size
- Celery Worker Termination Grace Period
- Extra Capacity*

![Astro UI Executor Config](https://assets2.astronomer.io/main/docs/astronomer-ui/Astro-UI-Executor.png)

Resource configurations are "live" as soon as you adjust the toggles on your web browser and click `Update`.

> **Note*:** Read below for details on configuring "Extra Capacity," as AU's allocated to this slider are both calculated and billed differently.

### The Astronomer Unit (AU)

To track and measure allocation to each of these components, we introduce the concept of [an Astronomer Unit (AU)](https://github.com/astronomer/houston-api/blob/193183be40df7e00261c3bf4941caf80e5d874a4/config/default.yaml#L317).

| AU Count | CPU | GB of Memory | Monthly Price |
|----------|-----|--------|-------|
| 1 | 0.1 | .375 | $10 |
| 10 | 1 | 3.75 | $100

When you spin up an Airflow deployment, you'll find that it's pre-configured with default resource allocations. We've identified those levels to be effective baselines for the Local, Celery and Kubernetes Executors, respectively. Of course, you're free to adjust them at any time.

See below for out-of-the-box configurations and corresponding AU count:

| Executor   | PgBouncer & StatsD | Scheduler | Webserver | Celery Worker | Redis & Flower | Extra Capacity | Total AU | Monthly Cost |
|------------|--------------------|-----------|-----------|---------------|----------------|----------------|----------|--------------|
| Local | 4 | 10| 5 | N/A | N/A | N/A | 19 | $190 |
| Celery | 4 | 10 | 5 | 10 | 4 | N/A | 33 | $330 |
| Kubernetes | 4 | 10 | 5 | N/A | N/A | 10 | 19 - 29 | $190 - $290  |

**Note**: The PgBouncer, StatsD, Redis, and Flower AU configs are static infrastructure minimums that cannot be changed.

### Extra Capacity

#### Kubernetes Executor & KubernetesPodOperator

On Astronomer, resources needed for either the [KubernetesPodOperator](/docs/cloud/stable/customize-airflow/kubepodoperator/) or the KubernetesExecutor are mapped to the `Extra Capacity` slider on your deployment's "Configure" page.

The number of AUs (as a combination of CPU and Memory) maps to [*resource quotas*](https://kubernetes.io/docs/concepts/policy/resource-quotas/) on the [Kubernetes Namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) in which your Airflow deployment lives.

At their core, both the Kubernetes Executor and KubernetesPodOperator dynamically spin up an individual Pod for each task that needs to be executed - and spin it down once that task is completed.

The total AU's allocated to the "Extra Capacity" slider represent the *maximum* possible resources that *could* be provisioned to a single or set of pods at any given time. It does *not* represent actual usage and will not be charged as a fixed resource.

#### Pricing

On Astronomer Cloud, you will only be charged for the CPU and Memory *actually used* by either your Kubernetes Executor or KubernetesPodOperator within the span of your billing period. The AU value you set for the slider represents the *maximum* AU our cluster has permission to provision within your deployment's namespace, but you will not be charged for that maximum unless fully utilized.

If you set your "Extra Capacity" slider to 30AU but the Pods spun up within your billing period only required 20AU total to execute all tasks, you will only be charged for 20AU at the end of the month (at our regular rate of $10/AU per month).

> Note: While "Extra Capacity" is dynamically priced, resources needed for the Scheduler, Webserver, PgBouncer & StatsD will remain fixed costs that are not affected by downtime/uptime.

#### Node Limits on Astronomer Cloud

On Astronomer Cloud, the node limits for any single task (based on Google's [standard-16 machine type](https://cloud.google.com/compute/docs/machine-types)) are:

- 58 GB of Memory/RAM
- 15 CPU

### Billing

To calculate your bill at the end of the month, we take a snapshot of your deployment's resource allocations every individual hour your deployment is running on our platform (via an Airflow DAG, of course).

At the end of the month, we aggregate the total AU hours for that billing cycle - and convert it to a dollar amount.

For example, the cost of running 1 deployment on 25 AUs (a standard deployment with 1 worker using the Celery executor) for 1 day = (25) x (0.0137) x (24).

> Note: Resources allocated to "Extra Capacity" are calculated and charged by the *minute* (via [kubecost](https://kubecost.com/)), not by the hour.

## Non-Profit Discount

If you're a not-for-profit interested in Astronomer, our team wants to support you and your mission.

On Astronomer, all organizations with a 501(c)(3) status are eligible for a 20% discount across all product offerings.

If you're interested and believe you might be eligible, reach out to us at humans@astronomer.io.

## Enterprise Pricing

If you're interested in Enterprise and haven't already talked to us, reach out [here](/contact).
