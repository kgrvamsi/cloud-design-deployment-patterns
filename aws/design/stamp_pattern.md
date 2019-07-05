# Pattern Code: SSAWSDSN03

## Pattern Name: Stamp Pattern

## Problem
 Setting up the OS and the applications for a virtual server takes just as much work, time and expense as for a physcial server.

 A Virtual Server is a server that does not depend on hardware.

 Without any major changes to the physical machine , the virtual machine can function as if it were a physical machine as this is very convenient that you can create or delete virtual machines immediately whenever the need arises.

 The setup required to use a virtual server still takes as much labor, time  and expense as for a virtual server.

## Explanation of the Pattern

We can use the AWS cloud to create a machine image in a state where the operating system, middleware and applications have already been set up on a virtual server, and use that image to launch a new virtual server.

Onca you have set up the operating system , middleware and applications , you can copy them for use at any time.

Copying the virtual servers like using a rubber stamp lets you generate virtual servers in large quantities , with the environments already set up and ready to go.

By using Aws Cloud , on the other hand , resources such as the servers,disks aare handled logically so we can perform operations easily.

## Implementation

If you create an Amazon Machine Image (AMI) from an Elastic Block Store (EBS) that includes the boot region of the operating system, you will be able to launch an EC2 instance from the AMI, making it possible for you to produce a large numbers of EC2 instances with identical settings.

###### (Procedure)

-- Launch an EC2 instance and install the required software.

-- Perform the necessary set up and create a state where it is running as a server.

-- Capture and save the AMI after confirming proper operation.

-- Use that AMI to create the required numbers of servers, when required.

## Configuration

![Stamp Pattern](https://cacoo.com/diagrams/2XNdewVsgellO3x8-3FAF6.png)

Pic Credits: http://en.clouddesignpattern.org

## Benefits

-- Using an AMI where the environment has already been constructed frees you from the operations required for setting up the EC2 instances launched based on that AMI.

-- You can launch hundreds of EC2 instances with identical operating systems, data, and settings.

-- Even when using a script to launch EC2 instances, set up operations are unnecessary if the environment has already been constructed for the AMI, making it possible to simplify the script.

-- Not only can you use the AMIs that have been constructed, but you can share the AMIs with specified users, and, if necessary, you can publish the AMIs.

## Cautions

-- The point in time at which the snapshot should be taken and the timing with which the AMIs should be updated have to be handled on a case-by-case basis . The way to do so will depend on the system requirements.

-- EC2 instances (virtual servers) that are exactly identical are replicated, so when there are items requiring changes in settings for individual virtual servers there is a need for some way to do so.

-- Once you create an AMI, any patches or revisions to the base operating system will not be implemented automatically in the AMI. You will have to perform patching and upgrading operations on the individual AMIs.

## Other

## Reference URL

[Click here](http://en.clouddesignpattern.org/index.php/CDP:Stamp_Pattern)