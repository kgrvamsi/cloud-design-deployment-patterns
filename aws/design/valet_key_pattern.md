# Pattern Code: SSAWSDSN02

## Pattern Name: Valet Key Pattern

## Problem

Client Programs and Web browsers often need to read and write the files or data streams to and from an application's storage.Typically the application will handle the movement of the data either by fetching it from storage and streaming from the client and storing it in the data store.

During this approach it absorbs valuable resources such as compute,memory and bandwidth.

Data stores have the ability to handle upload and download of data directly without requiring that the application perform any processing to move this data, but this typically requires the client to have access to the security credentials for the store.

This can be a useful technique to minimize data transfer costs and the requirement to scale out the application, and to maximize performance and it also means that the application is no longer able to manage security of the data.

After the client has a connection to the data store for direct access the application can't act as the gatekeeper, it's no longer in control of the process and can't prevent subsequent upload or downloads from the data store.

This isn't a realistic approach in distributed systems that need to serve untrusted clients, instead applications must be able to securely control access to data in a granular way, but still reduce the load on the server by setting up this connection and then allowing the client to communicate directly with the data store to perform the required read or write operations.

## Explanation of the Pattern

## Implementation

## Configuration

## Benefits

## Cautions

## Other

## Reference URL

[Click here](https://docs.microsoft.com/en-us/azure/architecture/patterns/valet-key)