# Cluster State Dependent Nagios Check Wrapper Plugin

This small Nagios plugin is intended to be used as a wrapper plugin on High-Availability hosts running Corosync/Pacemaker
or some other cluster software. The plugin checks for a running Pacemaker resource (using /usr/sbin/crm_resource) or
a local service ip address.
If the resource is active or the ip is present, it will execute the given nagios plugin and returns the state of the 
plugin. If the host is a inactive cluster host (resource or ip not present on the local host), it will simply output a
plugin state OK.

The plugin might be used on a Cluster, where plugins are executed through passive checks (for example using Check MK).

Other solutions like check_multi, Nagios service dependencies, Check MK cluster configuration, ... might also be
a solution, but will not work in every scenario (especially using passive checks).

This plugin allows to integrate automatic passive checks using a configuration management software (like Puppet) with 
no manual configuration of resources on the monitoring host.

The crm_resource need root privileges. The current implementation using a shell call might be improved, but works for us.
Be sure to also monitor your cluster state by some other mechanism.

# Dependencies

 * Python 2.7
 * netifaces

# Installation

```
python setup.py install
```

# Usage

Simply define a Nagios Check by adding the wrapper in front and give either a crm resource name or ip address as a parameter
to the script. The nagios plugin to be called including multiple parameters can be added after the resource or ip:

Example using a crm resource:

```
/usr/bin/check_cluster_state_dependent -r p_radius  /usr/lib/nagios/plugins/check_ping -H 127.0.0.1 -w 100,10% -c 200,20%    
```

Example using a ip:


```
/usr/bin/check_cluster_state_dependent -i 127.0.0.1  /usr/lib/nagios/plugins/check_ping -H 127.0.0.1 -w 100,10% -c 200,20%    
```

