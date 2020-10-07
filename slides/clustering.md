class: middle, center, blue

# Domain Clustering 

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

* Really need to build up health check in load balancer
* Needs to check that login to app is working

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
* Need a better health check

## App

* No need to expose JOLT ports 
* Disable JOLT compression 
    * Set maximum permitted value
        * `[JOLT Listener]\Jolt Compression Threshold=2,147,483,647`
    * Remove `-c` from JSL section of `psappsrv.ubx`

???

* Not easy to make psserver dynamic
* JSP check
