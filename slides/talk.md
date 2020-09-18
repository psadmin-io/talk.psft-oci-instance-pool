class: center, middle, blue

# OCI Instance Pool

---

# OCI Instance Pool

* Create and manage multiple compute instances that share the same configuration. 
* Ability to scale instance count and attach to a Load Balancer. 
* Multiple scaling methods:
    * Schedule
    * Metric based rules
    * Script
    * OCI Console

???

* Adjusting the instance count will create or terminate instances automatically. 
* Pool is attached to a load balancer, the instances are automatically added or removed
* Would be great in the PeopleSoft space
    * load can vary day-to-day
    * Think “timesheet day”
    * open enrollment
    * course registration

---

# What is needed?

1. Load Balancer
???
Create in OCI for instance pool automation to work
--
1. Instance Image
???
Custom or platform image, depending on auto prov. scripts (more later)
--
1. Automated Provisioning 
???
* DPK(puppet) 
* cloud-init scripts
    * Advanced Options > Mgmt > Initialization Script
    * Terraform - instance user_data

---

# Domain Clustering 

## What?

* Web domains (PIA)
* App domains
* ~~Batch domains~~

???

* Talking about midtier, specifically web and app
* PRCS can have multiple, but usually set hard at N domains
* Database is singular, HA through RAC or other measures

---

# Domain Clustering 

## Why?

* Load Balancing
* High Availability

???

* Spread load to multiple domains
* Ability to failover if a domain goes bad

---

# Domain Clustering 

## How?

* Web behind a Load Balancer
    * F5, a10, etc
* Web connects to App via JOLT
    * `configuration.properties`
        * `psserver=server01:9000, server02:9000`

???

* Multiple web servers placed behind a load balancer
* App connection string set in PIA site `configuration.properties`
* Can be load balanced, failover

---
class: center, middle, white

# Approaches to Clustering

???

Quick Poll - How familiar are you with clustering? What type do you use? TODO

---

# Web->Multi App

.center[`webServer01 - psserver=appServer01:9000, appServer02:9000`]
.center[`webServer02 - psserver=appServer01:9000, appServer02:9000`]
.center[![psadmin.io](images/cluster-multi.png)]

???

---

# Web->Single App, w/ Failover

.center[`webServer01 - psserver=appServer01:9000{appServer02:9000}`]
.center[`webServer02 - psserver=appServer02:9000{appServer01:9000}`]
.center[![psadmin.io](images/cluster-failover.png)]

???

---

# Web->Single App

.center[`webServer01 - psserver=appServer01:9000`]
.center[`webServer02 - psserver=appServer02:9000`]
.center[![psadmin.io](images/cluster-single.png)]

???

---

# Stand-alone Web/App

.center[`webServer01 - psserver=localhost:9000`]
.center[`webServer02 - psserver=localhost:9000`]
.center[![psadmin.io](images/cluster-webapp.png)]

???

---

# Stand-alone Web/App

.center[`webServer01 - psserver=localhost:9000`]
.center[`webServer02 - psserver=localhost:9000`]
.center[![psadmin.io](images/cluster-webapp-n.png)]

???

---

# Web/App Considerations

## Web

* Set `psserver=localhost:9000`
* Set `<cookie-name>ENVNAME-PORTAL-PSJSESSIONID</cookie-name>`

## App

* Disable JOLT compression 

---
class: center, middle, blue

# Approaches to Auto Provisioning

---

Talk about it

---
class: center, middle, gray

# Demo

---
class: center, middle, white

# Custom Image

---
class: center, middle, white

# Straight DPK

---
class: center, middle, white

# Shared Middleware

---
class: center, middle, white

# Custom Image