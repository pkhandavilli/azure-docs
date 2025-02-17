---
title: Requirements and considerations for Azure NetApp Files cross-zone replication | Microsoft Docs
description: Describes the requirements and considerations for using the volume cross-zone replication functionality of Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-ahibbard
manager: ''
editor: ''

ms.assetid:
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/15/2022
ms.author: anfdocs
---
# Requirements and considerations for using cross-zone replication 

This article describes requirements and considerations about [using the volume cross-zone replication](create-cross-zone-replication.md) functionality of Azure NetApp Files.

[!INCLUDE [Azure NetApp Files cross-zone-replication supported regions](includes/cross-zone-regions.md)]

## Requirements and considerations 

* The cross-zone replication feature uses the availability zone volume placement feature of Azure NetApp Files. The availability zone volume placement feature is currently in private preview. Contact your account team to request access to the availability zone volume placement feature.  
    * You can only use cross-zone replication in regions where the availability zone volume placement is supported.
* To establish cross-zone replication, the source volume needs to be created in an availability zone.  
* You can’t use cross-zone replication and cross-region replication together on the same source volume.
* SMB volumes are supported along with NFS volumes. Replication of SMB volumes requires an Active Directory connection in the source and destination NetApp accounts. The destination AD connection must have access to the DNS servers or AD DS Domain Controllers that are reachable from the delegated subnet in the destination zone. For more information, see [Requirements for Active Directory connections](create-active-directory-connections.md#requirements-for-active-directory-connections). 
* The destination account must be in a different zone from the source volume zone. You can also select an existing NetApp account in a different zone.  
* The replication destination volume is read-only until you fail over to the destination zone to enable the destination volume for read and write. For more information about the failover process, refer to [fail over to the destination volume](cross-region-replication-manage-disaster-recovery.md#fail-over-to-destination-volume).
* Azure NetApp Files replication doesn't currently support multiple subscriptions; all replications must be performed under a single subscription.
* See [resource limits](azure-netapp-files-resource-limits.md) for the maximum number of cross-zone destination volumes. You can open a support ticket to [request a limit increase](azure-netapp-files-resource-limits.md#request-limit-increase) in the default quota of replication destination volumes (per subscription in a region). 
* There can be a delay up to five minutes for the interface to reflect a newly added snapshot on the source volume.  
* Cascading and fan in/out topologies aren't supported.
* Configuring volume replication for source volumes created from snapshot isn't supported at this time.
* After you set up cross-zone replication, the replication process creates *SnapMirror snapshots* to provide references between the source volume and the destination volume. SnapMirror snapshots are cycled automatically when a new one is created for every incremental transfer. You cannot delete SnapMirror snapshots until replication relationship and volume is deleted. 
* You cannot mount a dual-protocol volume until you [authorize replication from the source volume](cross-region-replication-create-peering.md#authorize-replication-from-the-source-volume) and the initial [transfer](cross-region-replication-display-health-status.md#display-replication-status) happens.
* You can delete manual snapshots on the source volume of a replication relationship when the replication relationship is active or broken, and also after the replication relationship is deleted. You cannot delete manual snapshots for the destination volume until the replication relationship is broken.
* You can't revert a source or destination volume of cross-zone replication to a snapshot. The snapshot revert functionality is greyed out for volumes in a replication relationship. 

## Next steps
* [Understand cross-zone replication](cross-zone-replication-introduction.md)
* [Create cross-zone replication relationships](create-cross-zone-replication.md)
