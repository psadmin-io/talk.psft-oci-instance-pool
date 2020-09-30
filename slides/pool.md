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
* Amount needed depends on Image choice
* DPK(puppet) 
* cloud-init scripts
    * Advanced Options > Mgmt > Initialization Script
    * Terraform - instance user_data
--
1. Instance Configuration
???
All instance configuration details like network, OS, shape, and image needed to create an instance. 
--
1. Instance Pool
???
* Includes everything listed above
* Gives use the ability to pick our instance count

---

# What are the challenges?

1. Not designed for adding nodes at different tiers
--
1. Web domains pointing to multiple App domains
--
1. Config having unique values, like Domain IDs 
--
1. Configuration referencing specific hostnames
--
1. Downloading and installing PeopleTools takes TIME

???

Let's first dive into that first point and discuss how domain clustering works with PeopleSoft