---

copyright:
  years: 2020
lastupdated: "2020-09-11"

keywords: network components, IBM Cloud Kubernetes Service, backup, restore, disaster, peer, orderer, ordering node, LevelDB, CouchDB

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Backing up and restoring components and networks
{: #backup-restore}

There are a number of reasons why users might want to backup their components individually or the network generally. While it's a best practice to backup components before upgrading them to a new Fabric version, network backups can be useful for cases in which a flawed smart contract causes invalid transactions to be written to the ledger. While the development and testing cycle for a smart contract should catch these errors, it might be necessary to reinstate a previous version of the network before the invalid transactions were written.

The process described in this topic covers both scenarios (component backups are a subset of the more general process for backing up the network). However, it does not describe how to restore deleted components or environments.

##  Overview
{: #backup-restore-overview}

As with anything deployed to a Kubernetes-based cluster, backups of components and networks in the IBM Blockchain Platform is a matter of backing up what's known as [persistent volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}. These volumes are where ledgers and other types of storage are mounted so they can be used by nodes.

As a result, "backing up" a component or a network is the process of saving a copy of the relevant persistent volumes, while "restoring" a component or network involves bringing up components and pointing them to these saved volumes. The number of volumes associated with a node depends on the type of node. Peers will either have one persistent volume (if using LevelDB) or two persistent volumes (if using CouchDB), while each node in the ordering service will have a single persistent volume.

Because ordering nodes cannot pull blocks from peers, **all peers must be backed up before the ordering nodes**. This ensures that the peers will not have any blocks on their ledgers that don't appear on the ledgers of the backed up ordering nodes. If any peers have blocks that the ordering nodes do not have, the ledgers will not be able to be synchronized, which will cause the restoration of the network to fail. If you are backing up your ledger as part of an upgrade to a new Fabric version, you do not have to synchronize this backup with the rest of the network. However, make sure to keep at least one ledger backup that is synchronized with the rest of the network in case a larger network restoration is necessary.
{: important}

When backing up your peers, note that if you're using CouchDB, which is mounted in a separate pod than the peer, you should back up the persistent volume associated with CouchDB before the volume associated with the peer pod. This is because the peer will check on CouchDB after booting and if anything is missing from CouchDB, the peer can push transactions to it which CouchDB will build into the database. However, any transactions in the state database cannot be pushed back to the peer.

As a general rule, it is a good practice to schedule backups (also known as "snapshots") to happen at the same time across the network. For example, if all of the peers in the network are using CouchDB, schedule for all of the CouchDB persistent volumes to be backed up at the same time, followed by all of the peer persistent volumes (again, at the same time), and then the ordering node persistent volumes. For an example of a backup schedule a network might choose to adopt, check out [Example snapshot schedule](#backup-restore-example).

While it is a best practice to periodically back up your Certificate Authority, these backups do not have to be coordinated with the rest of the network. Note that if your CA uses a local database, it will have a single persistent volume. If you are using `PostGreSQL`, you will have an additional persistent volume associated with it that will also need to be backed up.

### Scheduling snapshots
{: #backup-restore-schedule-snapshot}

It is a best practice to regularly schedule snapshots of your components on a regular basis. If possible, try to coordinate these snapshots with the other members of your network to ensure that the most recent snapshots can be used should the need arise. Because no peer snapshots can be used that are more recent than ordering service snapshots, you will be bound by your snapshots of the ordering service when attempting to restore.

Even though snapshots will not disrupt the functionality of your nodes, try to schedule snapshots during a time when the fewest transactions are being made as it will ensure a minimum of difficulty should the need to restore from a snapshot arise. The more distributed the nodes in your network are, the more difficult it will likely be to schedule the snapshots during a downtime. If necessary, prioritize the synchronization of snapshots (snapshotting peers before ordering nodes), over relative lulls in transactions being pushed.
{:tip}

For this example, we'll assume one backup every 24 hours and we'll retain seven backups.

Peer pods:
- CouchDB snapshot daily at 3:00 A.M.
- Peer snapshot daily at 3:05 A.M.

Ordering node pods:
- Orderer snapshots daily at 5:00 A.M. The two hour gap will allow sufficient time for all of the peers to be snapshotted from every organization before snapshotting ordering nodes.

## Taking snapshots
{: #backup-restore-take-snapshot}

The overall steps for the backup and restore process apply in all cases and are independent of where your components are deployed. However, the specific commands used in this example for creating, listing, and restoring snapshots only apply if you are running on {{site.data.keyword.cloud_notm}}. For other cloud providers or when running in your own datacenter, you will need to follow those specific instructions for underlying snapshot creation and management.

### Peer snapshot
{: #backup-restore-peer-snapshot}

After your node have been provisioned, you'll be able to see the persistent volumes associated with it and the namespace the component has been deployed into by issuing:

```
kubectl get pvc --all-namespaces` or `kubectl get pvc -n <namespace>
```

You will see a result similar to this:

```
NA.M.ESPACE   NA.M.E                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
n3392fd     orderer1node1-pvc   Bound    pvc-34723fe1-c8e9-4fe7-bdf8-75b8a2f22912   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer2node1-pvc   Bound    pvc-11b34473-bfdf-4fac-962b-5999bb961a50   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer3node1-pvc   Bound    pvc-33ed8cce-21ec-44a8-8a0f-190052b985f9   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer4node1-pvc   Bound    pvc-5fd2b807-2361-406b-802a-a18dbaabe23e   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     orderer5node1-pvc   Bound    pvc-01da603e-3d2b-4b77-a0ac-af3e70e2d2f5   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     ordererorgca-pvc    Bound    pvc-e582ef7f-04ae-4493-9bdb-426c784886c3   20Gi       RWO            ibmc-file-bronze   49d
n3392fd     peera-pvc           Bound    pvc-def5a60c-25f3-4de0-b9cc-2cf8dcf88cc2   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     peera-statedb-pvc   Bound    pvc-494ba122-3501-4413-b719-cbb4b3a8f91b   100Gi      RWO            ibmc-file-bronze   49d
n3392fd     peerc-pvc           Bound    pvc-ba9cee42-4d35-48d9-ad05-85afc7256633   100Gi      RWO            ibmc-file-bronze   48d
n3392fd     peerc-statedb-pvc   Bound    pvc-e945df85-5a32-4d37-991d-b2be334bc09b   100Gi      RWO            ibmc-file-bronze   48d
n3392fd     peerorg1ca-pvc      Bound    pvc-152be78e-202d-43a4-bcf7-81a6a9013c2d   20Gi       RWO            ibmc-file-bronze   49d
nd473f0     orderer1node1-pvc   Bound    pvc-435f7371-c2ec-46af-b681-b2ba94cf1684   100Gi      RWO            ibmc-file-bronze   34d
nd473f0     orderer2node1-pvc   Bound    pvc-6ddeec9d-a7eb-4d72-90dc-e663cac7b376   100Gi      RWO            ibmc-file-bronze   34d
nd473f0     orderer3node1-pvc   Bound    pvc-6c44a497-ba1b-4514-bb8c-fce14c1469a1   100Gi      RWO            ibmc-file-bronze   33d
nd473f0     orderer4node1-pvc   Bound    pvc-76244c8d-5c20-4cb0-b0e5-1aedbcd8bc18   100Gi      RWO            ibmc-file-bronze   33d
nd473f0     ordererorgca-pvc    Bound    pvc-58fc0b9b-17d4-4591-a117-7afb195a9478   20Gi       RWO            ibmc-file-bronze   35d
```

To get the details on a particular volume, issue:

```
kubectl describe pv <VOLUME>
```

If you're using the CLI to manage snapshots, you'll need the `volumeId` value. If you are using the cluster UI, you'll need the `Username` Value. For an example volume named `pvc-494ba122-3501-4413-b719-cbb4b3a8f91b`, you would issue this command:

```
kubectl describe pv pvc-494ba122-3501-4413-b719-cbb4b3a8f91b
```

And see a result similar to:

```
Name:            pvc-494ba122-3501-4413-b719-cbb4b3a8f91b
Labels:          CapacityGb=100
                 Datacenter=dal10
                 Iops=2
                 StorageType=ENDURANCE
                 Username=IBM02SEV2046428_106
                 billingType=hourly
                 failure-domain.beta.kubernetes.io/region=us-south
                 failure-domain.beta.kubernetes.io/zone=dal10
                 path=IBM02SEV2046428_106data01
                 server=fsf-dal1002h-fz.adn.networklayer.com
                 volumeId=155617812
Annotations:     ibmFileProvisionerIdentity: 44cc1a5f-b755-11ea-bfae-3692beefed97
                 pv.kubernetes.io/provisioned-by: ibm.io/ibmc-file
Finalizers:      [kubernetes.io/pv-protection]
StorageClass:    ibmc-file-bronze
Status:          Bound
Claim:           n3392fd/peera-statedb-pvc
Reclaim Policy:  Delete
Access Modes:    RWO
VolumeMode:      Filesystem
Capacity:        100Gi
Node Affinity:   <none>
Message:
Source:
    Type:      NFS (an NFS mount that lasts the lifetime of a pod)
    Server:    fsf-dal1002h-fz.adn.networklayer.com
    Path:      /IBM02SEV2046428_106/data01
    ReadOnly:  false
Events:        <none>
```

You can then order snapshots for that volume, which can be done using either the CLI or the cluster UI. For information on how to take the snapshot, see [Ordering Snapshots](https://cloud.ibm.com/docs/FileStorage?topic=FileStorage-ordering-snapshots){: external}.

For our example, we'll use the CLI and order a single snapshot using 5GB of space by issuing:

```
ibmcloud sl file snapshot-order 155617812 -s 5
```

You may need more or less than 5GB of space, depending on the size of the pod and the resources that have been used on it.
{: tip}

You'll see a warning that charges will be incurred on your account. Press `y` to accept the charges and order the snapshot.

Alternatively, you can enable snapshots to be taken on a regular schedule for a particular volume. For information on how to set of a snapshot schedule, see
[Adding a Snapshot schedule](https://cloud.ibm.com/docs/FileStorage?topic=FileStorage-managingSnapshots#addschedule){: external}.

Again, for our example, we'll use the CLI to set up a daily snapshot of a CouchDB volume at 3:00 A.M. per our plan above and retain seven snapshots.

```
ibmcloud sl file snapshot-enable 155617812 -s DAILY -c 7 -r 3
```

You'll see a message similar to:

```
OK
DAILY snapshots have been enabled for volume 155617812.
```

We can now list the snapshot schedule to verify it's in place by issuing:

```
ibmcloud sl file snapshot-schedule-list 155617812
```

You'll see a response similar to:

```
id       active   type    replication   date_created           minute   hour   day   week   day_of_week   date_of_month   month_of_year   maximum_snapshots   
542130   *        DAILY   -             2020-09-02T16:35:58Z   0        3      *     -      *             *               *               7   
```

Repeat the following steps for each pod you want to back up remembering to schedule your backups in the proper order.

## Restoring nodes
{: #backup-restore-restore}

Here we have two examples. In the first example, we show the steps for restoring peer nodes, while in the second example, we describe the steps for restoring the ordering service, which will also require restoring peer nodes.

Because the details for finding the persistent volumes associated with each ordering node and restoring snapshots are identical to what is documented for the peer, the commands will only be listed once, in [the peer instructions](#backup-restore-restore-peer). However, there are additional considerations when restoring ordering nodes that are documented in [Restoring an ordering node](#backup-restore-restore-orderer).

### Sequencing restorations
{: #backup-restore-restore-sequence}

Just as when backing up, care must be taken to restore things in the proper order.

When restoring ordering nodes, even if you only restore one node, you will have to stop all of the pods for all the ordering nodes by scaling them down.

If every node must be restored, stop them all at once and then restore them all at once. Then make sure all of them come up and give them a few minutes to elect a leader and establish quorum before driving any transactions.

If you are restoring the ordering service from backup, you must also either create new peers or restore all peers to a backup older than the ordering service. The peers will then catch up to the latest data stored in the ordering service.

When restoring a peer, make sure to restore backups of CouchDB older than the backup of the peer pod. If your state database has transactions your peer does not have, the database will fail to sync with the peer.

### Restoring a peer
{: #backup-restore-restore-peer}

To restore a peer, you will need to find the deployment you want to restore and scale it down to zero to stop the node.

First, get the list of deployments:

```
kubectl get deployments
```

You will see a response similar to:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           1/1     1            1           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```

Then scale down the node. In this example, we'll scale down `peera`:

```
kubectl scale --replicas 0 deployment/peera
```

You'll receive a message similar to:

```
deployment.apps/peera scaled
```

Then look at your deployments again to make sure the peer has been scaled down:

```
kubectl get deployments
```

You'll receive a response similar to:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           0/0     0            0           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```

Note `peera` showing as not being ready.

Use the commands in the [Peer snapshot](#backup-restore-peer-snapshot) section to find the persistent volumes associated with the peer. Note that for a peer pod using CouchDB there will be two volumes.

For each volume, list the available snapshots for that volume. For information on how to list available snapshots, see [Listing all Snapshots with Space Used Information and Management functions](https://cloud.ibm.com/docs/FileStorage?topic=FileStorage-managingSnapshots#listing-all-snapshots-with-space-used-information-and-management-functions){: external}.

For our example, we'll use CLI and restore a specific snapshot from our volume ID for the CouchDB pod associated with `peera`:

```
ibmcloud sl file snapshot-list 155617812
```

You'll see a response similar to:

```
id          user_name             created                     size_bytes   notes   
167189176   IBM02SEV2046428_106   2020-09-03T03:00:00-05:00   143089      
```

Pick the snapshot you'd like to restore from the list, keeping in mind that the CouchDB backup must be older then the peer pod backup, and restore it. For information on how to restore the snapshot, see [Restoring storage volume to a specific point-in-time by using a snapshot](https://cloud.ibm.com/docs/FileStorage?topic=FileStorage-managingSnapshots#restoring-storage-volume-to-a-specific-point-in-time-by-using-a-snapshot){: external}.

The command will be similar to:

```
ibmcloud sl file snapshot-restore <volumeid> <snapshotid>
```

For our example we'll pick the one available snapshot from 3:00 A.M.:

```
ibmcloud sl file snapshot-restore 155617812 167189176
```

You'll see a response similar to:

```
OK
File volume 155617812 is being restored using snapshot 167189176.
```

When restoring a volume from a snapshot, any existing snapshots older than the one you're using to restore will be deleted as they no longer apply.
{: important}

Repeat the process for the other volume used by the peer container which can be found using the same commands. Choose the 3:05 A.M. snapshot corresponding to the 3:00 A.M. snapshot you restored for CouchDB volume.

Once both snapshots are restored, scale the deployment back up to one to redeploy the pod with the new data. First, check the status of the pods:

```
kubectl get deployments
```

You'll receive a response similar to:

```
NA.M.E            READY   UP-TO-DATE   AVAILABLE   AGE
ibp-operator    1/1     1            1           48d
orderer1node1   1/1     1            1           20h
orderer2node1   1/1     1            1           22h
orderer3node1   1/1     1            1           22h
orderer4node1   1/1     1            1           22h
orderer5node1   1/1     1            1           22h
ordererorgca    1/1     1            1           48d
peera           0/0     0            0           20h
peerc           1/1     1            1           20h
peerorg1ca      1/1     1            1           48d
```

Then scale the `peera` pod back up:

```
kubectl scale --replicas 1 deployment/peera
```

You'll receive a response similar to:

```
deployment.apps/peera scaled
```

Once the pod is back up, the peer has been restored. The peer channel membership, installed smart contracts, and ledger height will match what it had when the snapshot was taken.  Note that the peer will immediately start pulling new blocks from the ordering service or other peers once it comes up, so the ledger height may increase.

### Restoring an ordering node
{: #backup-restore-restore-orderer}

The process for restoring an ordering node is largely similar to the process for restoring a peer. However, there are a few additional things to keep in mind.

1. You must stop **all** the ordering nodes in the cluster by scaling down each deployment to zero before restoring **any** of the ordering nodes.
2. If you restore all ordering nodes, you must scale all the deployments up to 1 to restart them and give them five minutes to determine leadership before accepting any traffic. The easiest way to do this is to also stop the peers when restoring the ordering service.
3. You must restore **all** peers connected to that ordering service to a backup taken earlier than the ordering service backup you are restoring to, or provision all new peers.

In our example, the flow we would take is:

1.  Scale down all five ordering nodes and peer deployments to zero to stop all traffic on the network.
2.  Restore the peer nodes to the 3:00 A.M. CouchDB snapshot and the 3:05 A.M. peer pod snapshot.
3.  Restore each ordering node to it's 5:00 A.M. snapshot taken the same day as peer snapshots we just restored.
4.  Scale all five ordering service deployments up to 1 and wait for five minutes after they restarted to let them sync up.
5.  Scale all peer deployments up to 1.

