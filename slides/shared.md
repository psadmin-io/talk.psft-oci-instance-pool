
---
class: center, middle, white

# Shared Middleware

---

# Approach to Getting DPK Files

1. TODO - use `ioco` to download or use CM DPK repo

---

# Approach to Building Shared Middleware 

Leverage File Storage Services to use a read-only share for middleware installs

1. Create a FSS *File System*
--
1. Create a FSS *Mount Target*
--
1. Create Exports for `Read/Write` and `Read Only` usage
--
1. Create a new Instance and mount FSS share using `Read/Write` Export
--
1.  Create a new directory for desired middleware version
--
1. Link new middleware version directory to `/u01/app/oracle/product/pt`
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
