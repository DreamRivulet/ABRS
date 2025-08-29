# Backup Vault vs Recovery Service Vault

## Key Differences Table   

|  Feature  | Recovery Services Vault  | Backup Vault  |
| -------- | -------- | -------- |
|  Purpose  | Legacy + broad workload backup vault  | Newer, specialized backup vault  |   
|**Supports**           | Azure VM Backup, Azure Files, SQL in VM, SAP HANA, MARS, DPM/MABS | Azure Disk Backup, Azure Blob Backup, new workloads    |
| **Consistency**        | Supports app-consistent backups (VMs, SQL, SAP)          | Generally crash-consistent (disk/blob)                |
| **Retention**          | Short-term and long-term (years)                         | Short-term and long-term (policy-based)               |
| **Storage type**       | Hidden Azure storage in vault                            | Uses incremental snapshots / object versioning         |
| **Cross-region restore** | Supported (if GRS enabled)                             | Not available for all workloads yet                   |
Granularity**        | VM-level, file-level, app-level                          | Resource-level (disk, blob)                           |
| **Best for**           | Full VM/app workloads                                    | Specific resource backups                             |





# point-in-time restore

Point-in-Time Restore (PITR) is the ability to restore data to the exact state it was in at a specific moment in the past, within a defined retention window.

It’s like a “time machine” for your data — you pick a timestamp, and the system reconstructs the data as it existed at that moment.

Azure Examples: Azure Blob Storage (Operational Backup):  
- PITR lets you roll back a container to a specific time.
- Uses blob versioning + change feed + soft delete.
- Example:
  - If a user accidentally overwrites a blob at 10:05 AM, you can restore the container to 10:04 AM state.
 


## PITR vs Other Backup Types：  
| Feature  | 	Point-in-Time Restore	| Snapshot Restore  |
| --- | --- | --- | 
|  Time granularity	| Any second/minute in retention window  | 	Exact snapshot time  |
|  Backup frequency  | 	Continuous or near-continuous logs/versions  | 	On-demand or scheduled  |
|  Data loss (RPO)	|  Minimal	|  Up to snapshot interval  |
|  Storage location  | 	Often same system (operational)  | 	Same system or separate vault  |


# Recovery Point
A Recovery Point is a specific backup version of your data that you can restore from. It represents the state of the protected data at a particular time.  

## In Azure Backup
Azure Backup creates and retains recovery points based on your backup policy.  
Each recovery point is:
- Created from a full backup or incremental backup/snapshot
- Stored in the vault or storage account (depending on backup type)
- Marked with a timestamp (and sometimes a consistency type: crash-consistent, app-consistent)
  
## Example:
- You back up a VM daily at 02:00 and keep 30 days of backups.
- Azure Backup will maintain 30 recovery points.
- If you need to restore the VM to how it was on March 5 at 02:00, you pick the March 5 recovery point.
