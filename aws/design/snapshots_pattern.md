# Pattern Code: SSAWSDSN01

## Pattern Name: Snapshots Pattern

## Problem

Data needs to be safe and important which needs to be taken backup at regular intervals and needs to be automated instead any manual intervention.

## Explanation of the Pattern

Aws Cloud lets us use limitless capacity for web storage doing so safely with relatively little expense.

Aws Cloud uses the concept of "Snapshot" which is a backup copy of your data at a given point in time. It Lets you copy the data of a virtual server (including the operating system) along with other data, to Web storage easily to take these snapshots at regular intervals.You can take a snapshot with one click of a button in the Control Screen, or you can take a snapshot using an API.

You can Automate it using a program (Shell,Python etc) or using **Aws Batch** [<https://aws.amazon.com/batch/>] to take the snapshots on a regular periodic basis.

## Implementation

**_Amazon Elastic Block Store (EBS)_** which is the virtual storage in AWS has a snapshot function which you can take snapshot of your current instance using this function.

When you take a snapshot, it is stored in the **_Amazon Simple Storage Service (S3)_** **Object Storage** which is designed to have 99.999999999% durability.

When the EBS snapshot function is used, all of the data required to recreate the EBS volume is copied to S3.

A Snapshot that has been stored in S3 can be recovered as a new EBS volume and even if the data is lost or corrupted in the EBS you can recover the data from the time when the snapshot was taken.

When you use EBS as a data disk you can backup your data at any time by taking a snapshot.You can take as many new backups as necessary, whenever necessary, without having to worry about storage capacity.

When you use EBS as a boot disk , you can make a copy for each operating system, and store it as an Amazon Machine Image(AMI) and create a instance from that data.

## Configuration

![Snapshot Pattern](https://cacoo.com/diagrams/2XNdewVsgellO3x8-B8482.png)

Pic Credits: http://en.clouddesignpattern.org

## Benefits

-- Taking backups can be controlled by a computer program (Shell,Python etc) or using **Aws Batch** [<https://aws.amazon.com/batch/>] which can automate the process rather than having to make backups manually.

-- We can use **_Amazon Simple Storage Service (S3)_** which has high durability as the backup destination.

-- We can backup all of the data on the EBS volume as a snapshot enabling immediate use of the snapshot to create a new EBS volume where the recovery will be easy in the event of a failure.

-- We can make backups not just of user data but of each individual operating system as well.Becuase the backup for each OS is stored as an AMI you can launch new EC2 instances.

-- We can take data under specific circumstances(ex: after replacing an application or after updating data),and can take multiple generations, without having to worry about storage capacity.This lets you rebuild environment easily after a failure or a problem, enabling you to return to the environment of any given point in time.

## Cautions

-- We must maintain data consistency when taking snapshots.When you take a snapshot with the EBS volume mounted, make sure to take the snapshot in a state where logical consistency has been achieved, for example, after flushing the cache of the file system(EXT or NTFS), and after application transactions have been completed.

-- Typically, the smaller the data size of the boot disk, the more rapidly the virtual server can be started. Note that disk checks that are performed periodically (fsck in Linux) also take time.

## Other

-- We may want to split the boot partition and the data partition into separate EBS volumes when making a backup, because you will probably want to backup your data parts more often than your boot parts.

## Reference URL

[Click Here]

[Click here]: http://en.clouddesignpattern.org/index.php/CDP:Snapshot_Pattern
