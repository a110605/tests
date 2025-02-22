---
title: Verify that VMs stay up when disks are evicted
---

* Related issues
    - [#1334](https://github.com/harvester/harvester/issues/1334) Volumes fail with Scheduling Failure after evicting disc on multi-disc node
    - [#5307](https://github.com/harvester/harvester/issues/5307) Replicas should be evicted and rescheduled to other disks before removing extrak disk

## Verification Steps

1. Created 3 nodes Harvester.
1. Added formatted disk (called disk A) to node0 VM in the harvester node page.
1. Added disk tag `test` on following disk in the longhorn page.
    1. disk A of node0
    1. root disk of node1
    1. root disk of node2
1. Created storage class with disk tag `test`.
1. Created volume (called B) with previous storage class. You should check scheduling status in the longhorn page.
1. Created VM with volume B as extra volume (not boot volume).
1. The node scheduling status should look like this. {{< image "images/volumes/1334-image-01.png" >}}
1. Deleted disk A in the harvester node page, error message should be displayed.
1. Added disk tag `test` to **root disk of node0**.
1. Requested eviction and disabled scheduling on disk A in the longhorn dashboard.
    Before:
    {{< image "images/volumes/1334-image-02.png" >}} 
    
    After:
    {{< image "images/volumes/1334-image-03.png" >}}

1. Removed disk A in the harvester node page, and it should be removed.

## Expected Results
1. Disk A is removed on the harvester node page.
1. VM is running in whole steps.