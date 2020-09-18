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
1. Notify us

---

# Approaches to Provisioning

1. Custom Image
1. Shared Middleware
1. DPK Archives

---
class: center, middle, gray

# Demo

???

* Show the resources in console
* Login to each env, showing labeling on index.html page
* Increment the instance count on all 3

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

# Approach to Custom Image config

Create a custom image with fully installed PeopleSoft Web/App domains

1. Create an instance from base platform image
--
1. Deploy domains with DPK
--
1. Create `$PS_DOMAIN_NBR` environment variable
--
1. Update domain config to use `localhost`
--
1. Update domain config to use `$PS_DOMAIN_NBR`
--
1. Add domain configuration step to start service script 
    * `$PS_CFG_HOME/appserv/APPDOM/psft-appserver-APPDOM-domain-appdom.sh`
--
1. Save as a Custom Image

---
class: center, middle, gray

# Demo

???

* Show Instance Pool config
* Show off stuff on server
* Check on build

---
class: center, middle, white

# Shared Middleware

---

# Approach to Getting DPK Files

1. TODO - use `ioco` to download or use CM DPK repo

---

# Approach to Building Shared Middleware 

Create a block storage resource to use as a read-only share for middleware installs

1. Create a new Instance with a new block storage
--
1. Mount storage to `/u01/app/oracle/pt`
--
1. Deploy DPK with generic, non-version directory names
--
1. Detach storage and save, then terminate instance
--
1. Deploy new instances and attach storage as read-only
--
1. Deploy DPK with same generic, non-version directory names
--
1. For patching, repeat process with latest INFRA DPK and new storage
--
1. Swap read-only storage to new version on instances

???

* For patching, update provisioning with new storage, spin up Pool to use new storage, then terminate old
   * When terminating from pool, it deletes the oldest first - TODO

---
class: center, middle, gray

# Demo

???

* Show Instance Pool config
    * user_data
* Show off stuff on server
* Check on build

---
class: center, middle, white

# DPK Archives

---

# Approach to DPK Archives

Use custom or delivered INFRA DPK Archives to install middleware locally at instance creation

1. TODO

---
class: center, middle, gray

# Demo

???

* Show Instance Pool config
    * user_data
* Show off stuff on server
* Check on build

---
class: center, middle, white

# Review Timings

???

Have some query or something that shows timings