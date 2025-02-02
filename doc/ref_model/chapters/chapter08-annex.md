[<< Back](../../ref_model)
# 8 Annex
<!--<p align="right"><img src="../figures/bogo_ifo.png" alt="scope" title="Scope" width="35%"/></p>-->
<p align="right"><img src="../figures/bogo_sdc.png" alt="scope" title="Scope" width="35%"/></p>

## Table of Contents
* [8.1 Introduction.](#8.1)
* [8.2 Generic Test Cases.](#8.2)
* [8.3 Architecture specfiic test cases (if needed).](#8.3)
  * [8.3.1 OpenStack (OS).](#8.3.1)
  * [8.3.2 Kubernetes (K8s).](#8.3.2)

<a name="8.1"></a>
## 8.1 Introduction

This Chapter 8 Annex contains standard test cases to be executed furing NFVI valications to Ensure a Reference Implementation of CNTT Reference Model and CNTT Reference Architecture meets industry driven quality assurance standards for compliance and verification.  The OPNFV Verified Program (OVP), by Linux Foundation Networking (LFN), in partnership with the Compliance Verification Committee (CVC), will provide tracking and governance for RM/RA/RI verifications.

<a name="8.2"></a>
## 8.2 Generic Test Cases

The following test projects, and their respective test cases, will be executed with each RI implementation.  More targeted, or specific test cases, are listed below which may be a subset of test cases run within these test suites listed.


**FuncTest**

FuncTest test suites will be run as part of the validations, and as a pre-requisite with approximately 2000+ functional test cases leveraging Tempest plugins.  At a minimum, FuncTest executions will include reuse of functest-smoke (functional tests), functional-benchmarking (rally_full and rally_jobs).


- Additional Resources for FuncTest detailse:
 - Project Description: https://wiki.opnfv.org/display/functest/Opnfv+Functional+Testing#OpnfvFunctionalTesting-Testcases
 - User Guide: https://opnfv-functest.readthedocs.io/en/stable-hunter/testing/user/userguide/index.html
 - Overview of test suites: https://opnfv-functest.readthedocs.io/en/stable-hunter/testing/user/userguide/test_overview.html.


- Example, or Reference, Functest Status Reporting Artifacts:
 - Rally Verification (Status): http://artifacts.opnfv.org/functest/functest-opnfv-functest-smoke-iruya-tempest_full-run-259/results/tempest_full/tempest-report.html
 - Rally Tasks (Status): http://artifacts.opnfv.org/functest/functest-opnfv-functest-benchmarking-iruya-rally_full-run-169/results/rally_full/rally_full.html

<a name="8.3"></a>
## 8.3 Architecture Specific Test Cases (if needed)

<a name="8.3.1"></a>
### 8.3.1 OpenStack (OS)

<p><strong><u>Test Cases</u></strong></p>
<ul>
<li><span style="text-decoration: underline;"><strong>Baremetal - validations</strong></span>
<ul>
<li>**SUMMARY:** We will validate Control and compute nodes hardware, bios, Firmware (Pxe boot), interfaces configuration like SRIOV and also validating Grub, network bonding and mount points.
<li>Interface &ndash; Validate nic status for all member in bond1 group</li>
<li>Interface &ndash; MTU speed for bond1 interface</li>
<li>Grub &ndash; SR-IOV is enabled</li>
<li>Numa &ndash; Each server should configure with two numa boundaries
<ul>
<li>ODD numbered CPU/Cores are assigned to numa 1</li>
<li>EVEN numbered CPU/Cores are assigned to numa 0</li>
</ul>
</li>
<li>Numa &ndash; Ensure Total memory available is equally distributed between two numa boundaries</li>
<li>VF&rsquo;s &ndash;64 VFs being created on each PCI SYS Interface.</li>
<li>Modules &ndash; following modules loaded
<ul>
<li>bonding</li>
<li>8021q</li>
<li>i40e</li>
</ul>
</li>
<li>OS: IPTables are enabled.</li>
<li>OS: ntp is enabled and configured to sync system time</li>
<li>OS: Dedicate mount point for following file system
<ul>
<li>/var/log</li>
<li>/var/lib/nova</li>
</ul>
</li>
<li>OS: Validate Huge Pages are enabled, with proper config settings for the target infrastructure. (not available for all flavours)
<ul>
<li>Validate existence of, and setting for, Hugepage size (e.g. 1GB)</li>
<li>Validate the Number of Huge page (e.g. 320 &lt;Per server&gt;)</li>
</ul>
</li>
<li>OS: Validate proxy/iptables implementation
<ul>
<li>Reboot Server and ensure proxy starts up and all necessary route are added to system</li>
</ul>
</li>
<li>OS: Validate proxy/iptables implementation
<ul>
<li>Validate Boot order</li>
</ul>
</li>
<li>check NIC card is detect first than hard drive
<ul>
<li>Kernel version Comparison.</li>
</ul>
</li>
<li>Validate it matches the manifest version
<ul>
<li>Validate Bios.</li>
<li>Validate Firmware.</li>
<li>Validate Redfish tool and module is working.</li>
<li>CPU Performance mode validation</li>
<li>Compare CPU verification server specs</li>
</ul>
</li>
</ul>
</li>
</ul>
<ul>
<li><span style="text-decoration: underline;"><strong>VNF Interoperability - validations</strong></span>
<ul>
<li>**SUMMARY:** After VNF on boarded we are validating end to end openstack resources like Tenant, Network (L2/L3), CPU Pining, security policies, Affinity anti-affinity roles and flavours etc.
<li>Create Tenant</li>
<li>Create users</li>
<li>Assign role to user</li>
<li>Create security profile with appropriate ingress and egress rule</li>
<li>Load images to glance repository</li>
<li>Create Host aggregate with following meta data
<ul>
<li>xxx-ns-ha01 &ndash; metadata up=true</li>
<li>xxx-ns-ha02 &ndash; metadata cp=true</li>
<li>xxx-ns-ha03 &ndash; metadata tp=true</li>
<li>xxx-kvm-az01</li>
<li>xxx-kvm-az02</li>
<li>Assign Host aggregate based on meta data provided</li>
<li>Validate host assignment for each aggregate are per design</li>
<li>All server in ODD number RACK should be part of xxx-kvm-az01</li>
<li>All server in EVEN number RACK should be part of xxx-kvm-az02</li>
<li>Number of server in Host Agg in xxx-kvm-az01 should match xxx-kvm-az02</li>
</ul>
</li>
<li>Create minimum 5 flavour with following spec (can be independently validated)
<ul>
<li>cpu pining, Enable Huge Pages, cp=true</li>
<li>cpu pining, Enable Huge Pages, tp=true</li>
<li>cpu pining, Enable Huge Pages, up=true</li>
<li>cpu pining, Enable Huge Pages, up=true, cross numa boundaries (allowing VM to spin across numa boundaries)</li>
<li>Flavour without any extra spec.</li>
</ul>
</li>
<li>Create 3 Network and assigned appropriate subnet
<ul>
<li>Provider Network &ndash; SRIOV - VLAN (where applicable, allowing a device, such as a network adapter, to separate access to its resources among various PCIe hardware functions)</li>
<li>L3/Tenant Network</li>
<li>OAM Network</li>
</ul>
</li>
<li>Create routers across 2 tenant network (optional - i.e. create virtual router on two different tenants and validate the network connectivity between the two)</li>
<li>Validate anti-affinity and affinity rules</li>
<li>Validate user ability to force VM landing on given hypervisor host</li>
<li>Create VMs using flavour defined above and Attached ceph storage
<ul>
<li>Validate VM is able to extract meta data</li>
<li>Validate VM connectivity between SR-IOV Network</li>
<li>Validate SRIOV Port mapping to OS/VF (where applicable)</li>
<li>Validate VM connectivity between L3/Tenant network</li>
<li>Validate VM connectivity between L3/Network traffic passing through router.</li>
<li>Validate user-data script gets execute as part of POST VM creation in your stack</li>
<li>Assuming all above task is perform using heat template</li>
<li>Delete all Heat stack created as part of this testing</li>
<li>Validate VM&rsquo;s host-dev mapping to physical host</li>
</ul>
</li>
<li>Validate hairpinning Solution -- Communication between 2 VMs residing on same compute should not go over wire</li>
<li>Validate hairpinning Solution -- Communication between 2 VMs residing on same compute should not go over wire&nbsp;</li>
</ul>
</li>
</ul>
<ul>
<li><span style="text-decoration: underline;"><strong>Compute Component - validations</strong></span>
<ul>
<li>**SUMMARY:** Validate/Document VMs status and connectivity result after performing each of listed steps. Best candidate for this testing would be identify compute node that holds VMs which has l2 and l3 connectivity. Lag time between Shutdown and Startup should be no more than 10 minute
<ul>
<li>Restart libvirt pod</li>
<li>Restart nova-compute pod</li>
<li>Restart openvswitch-db pod</li>
<li>Restart openvswitch-vswitchd pod</li>
<li>Restart neutron-ovs-agent pod</li>
<li>Restart neutron-sriov-agent pod<span style="text-decoration: underline;">(where applicable)<strong><br /></strong></span></li>
</ul>
</li>
</ul>
</li>
<li><span style="text-decoration: underline;"><strong>Control Plane Component- validations</strong></span>
<ul>
<li>**SUMMARY:** We are validating RabbitMQ, Ceph, Mariadb and Openstack components like nova, glance, heat, keystone API and resillency test.
<li>Validate RabbitMQ resiliency by shutting down 1 or more pods. Make nova/openstack API call to see system result <br />(expected results is BAU)"</li>
<li>Validate nova-api resilency by shutting down 1 or more pods. Document API call results. (expected results is BAU)</li>
<li>Run similar resiliency test for each of listed services and expected result is BAU &ndash; No impact to VNF
<ul>
<li>nova-api-metadaa</li>
<li>nova-conductor</li>
<li>nova-scheduler</li>
<li>nova-placement-api</li>
<li>nova-console-auth</li>
<li>nova-novnc-proxy</li>
<li>nova-rabbitmq</li>
<li>glance-api</li>
<li>glance-registry</li>
<li>glance-rabbitmq</li>
<li>heat-api</li>
<li>heat-cfn</li>
<li>heat-engine</li>
<li>heat-rabbitmq</li>
<li>keystone-api</li>
<li>keystone-rabbitmq</li>
<li>keystone-rabbitmq</li>
</ul>
</li>
<li>Validate maridb cluster is insync
<ul>
<li>Studown mariadb and upon restart ensure its sync up with masterdb.</li>
<li>Maria DB is single point of failure</li>
</ul>
</li>
<li>Validate ceph
<ul>
<li>Restart ceph-osd</li>
<li>Document VNF impact while ceph is unavailable and once ceph service is restoredbeing restored.</li>
</ul>
</li>
</ul>
</li>
<ul>
<li><span style="text-decoration: underline;"><strong>Security - see Ch 7 for complete list</strong></span>
<ul>
<li>Validation above is performed using both RBAC Roles and User group policies, both of Admin and User Roles.
</ul>
<li>**SUMMARY:** Validate User Role to ensure it allow user to perform all designated task and prohibits user performing any unassigned task.</li>
<li>Validate Security Policy/Rules are enforced</li>
</ul>
</li>
</ul>
</ul>

<a name="8.3.2"></a>
### 8.3.2 Kubernetes (K8s)

TBD
