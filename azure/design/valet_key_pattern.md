# Pattern Code: SSAZREDSN01

## Pattern Name: Valet Key Pattern

## Problem

Client Programs and Web browsers often need to read and write the files or data streams to and from an application's storage.Typically the application will handle the movement of the data either by fetching it from storage and streaming from the client and storing it in the data store.

During this approach it absorbs valuable resources such as compute,memory and bandwidth.

Data stores have the ability to handle upload and download of data directly without requiring that the application perform any processing to move this data, but this typically requires the client to have access to the security credentials for the store.

This can be a useful technique to minimize data transfer costs and the requirement to scale out the application, and to maximize performance and it also means that the application is no longer able to manage security of the data.

After the client has a connection to the data store for direct access the application can't act as the gatekeeper, it's no longer in control of the process and can't prevent subsequent upload or downloads from the data store.

This isn't a realistic approach in distributed systems that need to serve untrusted clients, instead applications must be able to securely control access to data in a granular way, but still reduce the load on the server by setting up this connection and then allowing the client to communicate directly with the data store to perform the required read or write operations.

## Explanation of the Pattern

We need to resolve the problem of controlling access to a data store where the store can't manage authentication and authorization of clients.

One typical solution is to restrict access to the data store's public connection and provide the client with a key or token that the data store can validate.

This key or token is usually referred to as a **Valet Key** and it provides time-limited access to specific resources and allows only predefined operations such as reading and writing to storage or queues, or uploading and downloading in a web browser.

Applications can create and issue valet keys to client devices and web browsers quickly and easily, allowing clients to perform the required operations without requiring the application to directly handle the data transfer.

This removes the process overhead, and the impact on performance and scalability from the application and the server.

The client uses this token to access a specific resource in the data store for only a specific period and with specific restrictions on access permissions.After the specified period, the key becomes invalid and won't allow access to the resource.

![Valet Key Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/_images/valet-key-pattern.png)

It's also possible to configure a key that has other dependencies, such as the scope of the data.

The key can also be invalidated by the application.This is a useful approach if the client notifies the server that the data transfer operation is complete.The server can then validate that key to prevent further access.

Using this pattern can simplify managing access to resources because there's no requirement to create and authenticate a user, grant permissions, and then remove the user again.

It also makes it easy to limit the location, the permission, and the validity period all by simply generating a key at runtime.

The Important factors are to limit the validity period and especially the location of the resource, as tighlty as possible so that the recipient can only use it for the intended purpose.

## Implementation

![Valet Key Pattern](https://www.feval.ca/img/2019/sas/sas.jpg)

##### Process Flow

-- User requests access to upload a file for processing
-- Application checks the request (authentication and authorization)
-- Application provides a temporary access token that grants the user appropriate permissions to perform the requested action
-- User interacts with the protected service to perform the desired action

## Configuration

![SAS Configuration](https://docs.microsoft.com/en-us/azure/storage/common/media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)

Microsoft Azure supports **Shared Access Signatures(SAS)** on Azure Storage for granular access control to data in blobs, tables and queues and for Service Bus queues and topics allowing clients to perform then required operations without requiring the application to directly handle the data transfer.

The client uses this token to access a specific resource in the data store for only a specific period, and with specific restrictions on access permissions.

###### Client side Implementation:

[Click Here](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1#sas-examples)

## Benefits

-- To minimize resource loading and maximize performance and scalability.

-- Using a valet key doesn't require the resource to be locked, no remote server call is required, there's no limit on the number of valet keys that can be issued, and it avoids a single point of failure resulting from performing the data transfer through the application code.

-- Creating a valet key is typically a simple cryptographic operation of signing a string with a key.

-- To minimize operational cost , enabling direct access to stores and queues is resource and cost efficient can result in fewer network round trips, and might allow for a reduction in the number of compute resources required.

-- When clients regularly upload or download data, particularly where there's a large volume or when each operation involves large files.

-- When the Application has limited compute resources available, either due to hosting limitations or cost considerations for this scenario the pattern is even more helpful if there are many concurrent data uploads or downloads because it relieves the application from handling the data transfer.

-- When data is stored in a remote data store or a different datacenter.

-- If the application was required to act as a gatekeeper, there might be a charge for the additional bandwidth of transferring the data between datacenters, or across public or private networks between the client and the application and then between the application and the data store.

## Cautions

-- If the application must perform some task on the data before it's stored or before it's sent to the client like if the application needs to perform validation,log access success or execute a transformation on the data.

-- Some data stores and clients are able to negotiate and carry out simple transformations such as compression and decompression.

-- If the design of an existing application makes it difficult to incorporate the pattern but using this pattern typically requires a different architectural approach for delivering and receiving data.

-- If it's necessary to maintain audit trails or control the number of times a data transfer operation is executed and the valet key mechanism in use doesn't support notifications that the server can use to manage these operations.

-- If it's necessary to limit the size of the data, especially during upload operations. The only solution to this is for the application to check the data size after the operation is complete, or check the size of uploads after a specified period or on a scheduled basis.

## Other

A sample that demonstrates this pattern is available on [GitHub](https://github.com/mspnp/cloud-design-patterns/tree/master/valet-key).

## Reference URL

[Click Here](https://docs.microsoft.com/en-us/azure/architecture/patterns/valet-key)

[How SAS (Shared Access Signature) Works](https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1)