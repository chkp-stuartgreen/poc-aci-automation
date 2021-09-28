# Ansible playbooks for ACI POCs

These playbooks contain some sample commands that can be adapted for use in a POC or test environment around a sample enterprise scenario.

- Create a domain for a customer
- Create a virtual system for a customer based on physical or virtual hardware
- Configure a Cisco APIC Data Center object
- Create dynamic policy objects given a known name for an EPG within the ACI environment
- Add these to a policy
- Enable Identity Awareness for use by CloudGuard Controller
- Add a traditional host based rule
- Then tear it all down again

These playbooks can be run manually or integrated into a platform like Rundeck.
They could also be triggered as part of a CI/CD approach using Gitlab. A sample pipeline job is contained in the repository.

You will need to pass through a number of variables to the playbook:


Variable Name|Value
---|---
api_key|Management API key
apic_password|Password for connecting to the Cisco APIC
apic_url| URL / IP for the APIC. Eg "https://172.1.2.3"
apic_user_name|Username to access the APIC API
dc_object|Name for the DC object (APIC)
dc_uri_obj_1|URI in APIC for EPG1 to be created initially 
dc_uri_obj_2|URI in APIC for EPG2 to be created initially
dc_uri_new_obj|URI in APIC for an additional EPG to be added in a later phase (playbook 3)
mds_password|Check Point MDS password
mds_user|Check Point MDS user
new_domain| Name of the new customer domain (DMS/CMA)
new_domain_ip| IP address for the new domain
new_host_ip|IP for the static host created in play 4
new_host_name| Name for the above host
vs_interface_dot_vlan|Name of the interface and VLAN for the VS to be created. Eg, bond1.100
vs_ip_slash_masklength|IP and CIDR mask for the VS
vs_name|Name for the virtual system. Must be unique on the physical host.
vsx_host_name|Name of the object in the MDS/DMS which is hosting the VS (VS0)

When you run the playbook, you need to use pass in the variables with the -e parameter as "key=value" pairs (INI format in Ansible terms). For testing, it's easier to provide the whole block of variables to each command but you can of course specify only the values which are required for each play.

```bash
ansible-playbook -i hosts playbooks/1_create_mds_vs.yml -e "YOURKEY1=YOURVALUE1 YOURKEY2=YOURVALUE2"

ansible-playbook -i hosts playbooks/2_configure_policy.yml -e "YOURKEY1=YOURVALUE1 YOURKEY2=YOURVALUE2"

ansible-playbook -i hosts playbooks/3_add_new_epg.yml -e "YOURKEY1=YOURVALUE1 YOURKEY2=YOURVALUE2"

ansible-playbook -i hosts playbooks/4_classic_host_rule.yml -e "YOURKEY1=YOURVALUE1 YOURKEY2=YOURVALUE2"

ansible-playbook -i hosts playbooks/999_teardown.yml -e "YOURKEY1=YOURVALUE1 YOURKEY2=YOURVALUE2"
```