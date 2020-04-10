# Github Toolchest contains audit/config procedures, templates & utilities that streamline support of mission critical Storage, Hosting, Network and Data Base IT environments.  
Platforms: EMC (VMAX & Powermax), Netapp (A800, A700, 8xxx), Pure (Flash Array), Cisco (MDS & UCS), Oracle and VDBench.
Each of the procedures, templates & utilities inside this repository are internally documented.  Repository summary:

asmlist_r003.txt - Audit Oracle block storage envs (correlates ASM Instances, Groups & Disks with backend storage arrays).

dirlilst_r004.txt - Audit EMC [PV]MAX frontend FAs (Fibre Adapters).  Returns FA WWPNs & related Director Bit settings.

esxcfg-mpath_format_proc_r001.txt - Formats VMWare ESXi host "esxcfg-mpath -L" command output.

hbalist_r006.txt - Audit server [v]HBA ([virtual] Host Bus Adapters).  Supports physical RHEL, HP/UX & Solaris servers.

magic_rings_lunid_decode_encode_proc_r004.txt - Audit IEEE Format_6 external LUNIDs for EMC [PV]MAX & Netapp SAN.

mds_port_channel_audit_proc_r007.txt - Creates Cisco MDS Port Channel Summary Table for user defined MDS switch set.

mds_zone_extract_proc_r002.txt - Returns zone definitions for set of user defined search strings (includes Regex support).

metlist_r006.txt - EMC SRDF Metro migration "audit/config" utility (tracks migrations & creates host cleanup syntax).

nbinccel_r004.txt - utility audits Netbackup Includes for NDMP backups performed on EMC Celerra NAS storage.

netlist_r004.txt - Netapp NAS storage oversubscription audit utilitys (supports user defined threshhold alerting).

rdflist_r004.txt - EMC SRDF remote replication audit utility (supports configurable sliding scale "YYYY_MM" usage reporting).

vdblist_utility_&_templates_r005.zip - VDBench Storage Perf Benchmark utility & templates (supports "side_by_side" graphs).

vmax_audit_r006.txt - Audits EMC [PV]MAX (single line "Config, Pool, Efficiency, Provision & Subscription data per array). 
