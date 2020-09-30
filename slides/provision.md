class: center, middle, blue

# Automated Provisioning

---

# Automated Provisioning with Instance Pools

To leverage OCI Instance Pools we need some automated provisioning. This is what allows us to dynamically spin up new instances to add to our pool.

---

# Goals

1. Fully deploy a Web/App server
1. Automatically add\remove from Load Balancer
1. Configure unique values at instance creation
1. Complete as quickly as possible
1. Trigger deployments
    * Manual
    * Schedule
    * Metric

---

# Approaches to Provisioning

1. Custom Image
1. cloud-init

---
class: center, middle, gray

# Demo

???

* Instance Pools 
    * explain image and init 
        * pool with hostname, same DB
        * show index.html not responding (Bad Gateway)
    * Start
    * Show details, instance count, config, etc
    * Show work requests
    * Show instances (junk name, better FQDN)
* Load Balancer
    * Single LB w/ single Public IP
    * Hostnames
    * Listeners
    * Backend Sets > Backend
* Login to PIA
    * show index.html
    * show hostname, LB cookie
    * Login
* Increment the instance count to 3
    * image
    * init

---
class: center, middle, white

# Custom Image

---

# Approach to unique values

## $PS_DOMAIN_NBR

* Export environment variable in the psadm2 `~/.bashrc`
* Have this generate a unique number that can be used in your domain configuration
* Instances get a random number appended to the hostname
    * Example: `webapp-634836`, `webapp-973329`, etc.
* Grab this number for `$PS_DOMAIN_NBR`
```bash
$ export PS_DOMAIN_NBR=$(hostname | rev | cut -d- -f1 | rev)
$ echo $PS_DOMAIN_NBR
634836
```

---

# Custom Image config

Create a Custom image with Web/App domains

1. Create an instance from base platform image
???
* This can be any image. 
* Update it with any OS level prep you want.
--
1. Deploy domains with DPK
???
* Any deployment method you want here. 
* Easiest to run DPK. Do manual steps after, if you have any.
--
1. Add `$PS_DOMAIN_NBR` environment variable to `.bashrc`  
???
Add your "domain nbr" logic to an export variables in psadm2's bash profile  
--
1. Update Cookie Configuration in `weblogic.xml`
???
* Default PIA install puts hostname and port in cookie-config
* Set this to something ENV specfic, not node
--
1. Update PIA to use `psserver=localhost:9033`
???
* Set this to localhost, since these are self contained web\apps
--
1. Update `setEnv.sh` to use `ADMINSERVER_HOSTNAME=${HOSTNAME}`
???
* Default PIA install puts the literal hostname here
* Change to use dynamic HOSTNAME variable, maybe localhost?
--
1. Update `psappsrv.cfg` to use `Domain ID=APPDOM%PS_DOMAIN_NBR%`
???
* To get us a node specfic domain ID, leverage the DOMAIN_NBR variable
* NOTE: At first "configure", this will convert to a literal
* Another approach could be to mess with the .ubx file, but this seems easier
--
1. Add domain configuration step to start service script 
    * `$PS_CFG_HOME/appserv/APPDOM/psft-appserver-APPDOM-domain-appdom.sh`
    * `... $APPSRV_PS_HOME/bin/psadmin -c configure -d $APPSRVDOM" ...`
???
* When UBBGEN process runs, it has some specific references to hostname
* Adding a configure in front of the start service ensures these update on node creations
--
1. Cleanup `/etc/hosts`
???
* Remove the current hostname from file
* Now ready to create the custom image

---
class: center, middle, gray

# Demo

???

* Show off stuff on "pre-image" server
    * .bashrc 
    * `export PS_DOMAIN_NBR=629714`
    * Cookie
    * psserver
    * Domain ID
    * service
    * /etc/host
* Show Instance Pool status
* Login to image
* Delete cookies to show different machines

---
class: center, middle, white

# cloud-init

---

# Approach to cloud-init

Use `cloud-init` to setup and run DPK install

1. TODO

---

# ioco
## psadmin.io Cloud Operations Utility

A python utility for common tasks related to administering PeopleSoft in the Cloud

[github.com/psadmin-io/ioco](https://github.com/psadmin-io/ioco)

---

# Examples

* `ioco oci block --make-file-system --mount`
* `ioco dpk deploy --dpk-type=FSCM`

???

* Create a default partition and file system on new block storage, then mount it
* Get DPK files from CM dpk files repo, then deploy a midtier setup

---
class: center, middle, gray

# Demo

???

* Create new instance and show cloud-init section
    * Demo of cloud-init - TODO
* Show Instance Pool status
* Login to init
* Delete cookies to show different machinces
* Login to new demo instance show cloud-init results
    * cloud-init log - TODO
    * cloud-init scripts - TODO
    * ioco 
        * conf
        * cm_dpk_files usage
        * logs        
