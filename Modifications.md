## vault secret: 
- you need to have vault-secret file in the digitransit-ansible directory
## ride2go-inventory: 
- you should change the ip address according to the address of the machine you want to install it on.
## MakeFile:
- new command for running digitransit.yml with custom inventory
## digitransit.yml 
 - commented out every role except `base` and `digitransit` 
## variables
- created `vars/main.yml` for both `base` and `digitransit` role and assigned variables needed (with dummy content until changed)
## dependencies
- commented out geerlingguy.certbot dependencies and requirements
## docker-compose
- removed unneccessary parts related to the ui
## digitransit/main.yml
- commented out parts relating to the ui
## docker-compose.yml
- deleted encryption key and the UI part (everything between "services:" and "network:")
## all.yml
- commented out vaulted variables (they are unused variables, and causes encryption error)
- disabled unused services (prediction, gtfs, etc..)
## roles/base/main.yml
- removed send to matrix script
## docker-compose.yml
- changed image to `mfdz/opentripplanner:latest` (from d38...)
- changed otp_docker_tag to latest aswell.

