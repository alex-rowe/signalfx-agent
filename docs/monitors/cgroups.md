<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# cgroups

Monitor Type: `cgroups` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/cgroups))

**Accepts Endpoints**: No

**Multiple Instances Allowed**: Yes

## Overview

Reports statistics about cgroups on Linux.  This only supports cgroups v1
and not the newer v2 unified implementation.

For general information on cgroups, see http://man7.org/linux/man-pages/man7/cgroups.7.html.

For detailed information on `cpu` cgroup metrics, see [Red Hat's guide to CPU management](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/sec-cpu). Many of the metric descriptions come from that document. Note that the `cpuacct` cgroup is primarily an informational cgroup that gives detailed information on how long processes in a cgroup used the CPU.

For detailed information on `memory` cgroup metrics, see [Red Hat's guide to the Memory cgroup](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/sec-memory). Many of the metric description come from that document.  Also refer to the Linux Kernel's [memory cgroup document](https://www.kernel.org/doc/Documentation/cgroup-v1/memory.txt).

### Filtering
You can limit the cgroups for which metrics are generated with the
`cgroups` config option to the monitor.

For example, the following will only monitor docker generated cgroups:

```yaml
monitors:
 - type: cgroups
   cgroups:
    - "/docker/*"
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: cgroups
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.md#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `cgroups` | no | `list of strings` | The cgroup names to include/exclude, based on the full hierarchy path. This is an [overridable set](https://docs.signalfx.com/en/latest/integrations/agent/filtering.html#overridable-filters). If not provided, this defaults to all cgroups. E.g. to monitor all Docker container cgroups, you could use a value of `["/docker/*"]`. |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


#### Group cpu
All of the following metrics are part of the `cpu` metric group. All of
the non-default metrics below can be turned on by adding `cpu` to the
monitor config option `extraGroups`:
 - ***`cgroup.cpu_cfs_period_us`*** (*gauge*)<br>    The period of time in microseconds for how regularly a cgroup's access to CPU resources should be reallocated
 - ***`cgroup.cpu_cfs_quota_us`*** (*gauge*)<br>    The total amount of time in microseconds for which all tasks in a cgroup can run during one period.  The period is in the metric `cgroup.cpu_cfs_period_us`.
 - ***`cgroup.cpu_shares`*** (*gauge*)<br>    The relative share of CPU that this cgroup gets.  This number is divided into the sum total of all cpu share values to determine the share any individual cgroup is entitled to.

 - `cgroup.cpu_stat_nr_periods` (*cumulative*)<br>    Number of period intervals that have elapsed (the period length is in the metric `cgroup.cpu_cfs_period_us`)
 - `cgroup.cpu_stat_nr_throttled` (*cumulative*)<br>    Number of times tasks in a cgroup have been throttled
 - ***`cgroup.cpu_stat_throttled_time`*** (*cumulative*)<br>    The total time in nanoseconds for which tasks in a cgroup have been throttled

#### Group cpuacct
All of the following metrics are part of the `cpuacct` metric group. All of
the non-default metrics below can be turned on by adding `cpuacct` to the
monitor config option `extraGroups`:
 - ***`cgroup.cpuacct_usage_ns`*** (*cumulative*)<br>    Total time in nanoseconds spent using any CPU by tasks in this cgroup
 - `cgroup.cpuacct_usage_system_ns` (*cumulative*)<br>    Total time in nanoseconds spent in system (kernel) mode on any CPU by tasks in this cgroup
 - `cgroup.cpuacct_usage_user_ns` (*cumulative*)<br>    Total time in nanoseconds spent in user mode on any CPU by tasks in this cgroup

#### Group cpuacct-per-cpu
All of the following metrics are part of the `cpuacct-per-cpu` metric group. All of
the non-default metrics below can be turned on by adding `cpuacct-per-cpu` to the
monitor config option `extraGroups`:
 - `cgroup.cpuacct_usage_ns_per_cpu` (*cumulative*)<br>    Total time in nanoseconds spent using a specific CPU (core) by tasks in this cgroup.  This metric will have the `cpu` dimension that specifies the specific cpu/core.

 - `cgroup.cpuacct_usage_system_ns_per_cpu` (*cumulative*)<br>    Total time in nanoseconds spent in system (kernel) mode on a specific CPU (core) by tasks in this cgroup.  This metric will have the `cpu` dimension that specifies the specific cpu/core.

 - `cgroup.cpuacct_usage_user_ns_per_cpu` (*cumulative*)<br>    Total time in nanoseconds spent in user mode on a specific CPU (core) by tasks in this cgroup.  This metric will have the `cpu` dimension that specifies the specific cpu/core.


#### Group memory
All of the following metrics are part of the `memory` metric group. All of
the non-default metrics below can be turned on by adding `memory` to the
monitor config option `extraGroups`:
 - ***`cgroup.memory_failcnt`*** (*cumulative*)<br>    The number of times that the memory limit has reached the `limit_in_bytes` (reported in metric `cgroup.memory_limit_in_bytes`).

 - ***`cgroup.memory_limit_in_bytes`*** (*gauge*)<br>    The maximum amount of user memory (including file cache).  A value of `9223372036854771712` (the max 64-bit int aligned to the nearest memory page) indicates no limit and is the default.

 - `cgroup.memory_max_usage_in_bytes` (*gauge*)<br>    The maximum memory used by processes in the cgroup (in bytes)
 - `cgroup.memory_stat_active_anon` (*gauge*)<br>    Bytes of anonymous and swap cache memory on active LRU list
 - `cgroup.memory_stat_active_file` (*gauge*)<br>    Bytes of file-backed memory on active LRU list
 - ***`cgroup.memory_stat_cache`*** (*gauge*)<br>    Page cache, including tmpfs (shmem), in bytes
 - `cgroup.memory_stat_dirty` (*gauge*)<br>    Bytes that are waiting to get written back to the disk
 - `cgroup.memory_stat_hierarchical_memory_limit` (*gauge*)<br>    Bytes of memory limit with regard to hierarchy under which the memory cgroup is
 - `cgroup.memory_stat_hierarchical_memsw_limit` (*gauge*)<br>    The memory+swap limit in place by the hierarchy cgroup
 - `cgroup.memory_stat_inactive_anon` (*gauge*)<br>    Bytes of anonymous and swap cache memory on inactive LRU list
 - `cgroup.memory_stat_inactive_file` (*gauge*)<br>    Bytes of file-backed memory on inactive LRU list
 - `cgroup.memory_stat_mapped_file` (*gauge*)<br>    Bytes of mapped file (includes tmpfs/shmem)
 - `cgroup.memory_stat_pgfault` (*cumulative*)<br>    Total number of page faults incurred
 - `cgroup.memory_stat_pgmajfault` (*cumulative*)<br>    Number of major page faults incurred
 - `cgroup.memory_stat_pgpgin` (*cumulative*)<br>    Number of charging events to the memory cgroup. The charging event happens each time a page is accounted as either mapped anon page(RSS) or cache page(Page Cache) to the cgroup.

 - `cgroup.memory_stat_pgpgout` (*cumulative*)<br>    Number of uncharging events to the memory cgroup. The uncharging event happens each time a page is unaccounted from the cgroup.

 - ***`cgroup.memory_stat_rss`*** (*gauge*)<br>    Anonymous and swap cache, not including tmpfs (shmem), in bytes
 - `cgroup.memory_stat_rss_huge` (*gauge*)<br>    Bytes of anonymous transparent hugepages
 - `cgroup.memory_stat_shmem` (*gauge*)<br>    Bytes of shared memory
 - `cgroup.memory_stat_swap` (*gauge*)<br>    Bytes of swap memory used by the cgroup
 - `cgroup.memory_stat_total_active_anon` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_active_anon` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_active_file` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_active_file` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_cache` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_cache` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_dirty` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_dirty` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_inactive_anon` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_inactive_anon` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_inactive_file` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_inactive_file` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_mapped_file` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_mapped_file` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_pgfault` (*cumulative*)<br>    The equivalent of `cgroup.memory_stat_pgfault` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_pgmajfault` (*cumulative*)<br>    The equivalent of `cgroup.memory_stat_pgmajfault` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_pgpgin` (*cumulative*)<br>    The equivalent of `cgroup.memory_stat_pgpgin` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_pgpgout` (*cumulative*)<br>    The equivalent of `cgroup.memory_stat_pgpgout` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_rss` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_rss` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_rss_huge` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_rss_huge` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_shmem` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_shmem` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_swap` (*gauge*)<br>    Total amount of swap memory available to this cgroup
 - `cgroup.memory_stat_total_unevictable` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_unevictable` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_total_writeback` (*gauge*)<br>    The equivalent of `cgroup.memory_stat_writeback` that also includes the sum total of that metric for all descendant cgroups
 - `cgroup.memory_stat_unevictable` (*gauge*)<br>    Bytes of memory that cannot be reclaimed (mlocked, etc).
 - `cgroup.memory_stat_writeback` (*gauge*)<br>    Bytes of file/anon cache that are queued for syncing to disk

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.

## Dimensions

The following dimensions may occur on metrics emitted by this monitor.  Some
dimensions may be specific to certain metrics.

| Name | Description |
| ---  | ---         |
| `cgroup` | The name of the cgroup being described.  The name of a cgroup is the full relative path of the cgroup based on the cgroup controller's root directory. |
| `cpu` | For metrics that end with `_per_cpu`, this dimension will indicate which cpu the time series refers to. |



