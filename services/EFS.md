# Amazon Elastic File System (EFS)
Serverless, fully elastic file storage

## Useful links
- [AWS - EFS FAQ](https://aws.amazon.com/efs/faq/)


## Introduction
- Managed NFS (Network file system) that can be mounted on **many EC2**
- Works **Multi-AZ**
- Highly available, scalable, expensive (3 times the cost of GP2), pay per use
- Uses security group to control access to EFS
- **Only compatible with Linux based AMI**
- use case: content management, web serving, data sharing, Wordpress


## Performance & Storage Classes
- **EFS Scale**
    - 1000s of concurrent NFS clients, 10GB+ throughput
    - Grow to Petabyte-scale network file system, automatically
    
- **Performance Classes**
    - **Performance Mode (Set at EFS creation time)**
        - **General Purpose (default)**: Latency-sensitive use cases (web server, CMS, etc)
        - **Max I/O** higher latency, higher throughput, highly parallel (Big data, media processing)
    
    - **Throughput Mode**
        - **Bursting**: 1TB = 50MB/s + burst of up to 100MB/s
        - **Provision**: set your throughput regardless of storage size, ex: 1GB/s for 1TB storage 
        - **Elastic**: Automatically scale throughput based on workloads
- **Storage Classes**
    - **Standard**: For frequently accessed files
    - **Infrequent Access**: Cost to retrieve file but lower price to store. Enable with *Lifecycle Policy*
- **Availability and durability**
    - **Standard**: Multi-az, great for prod
    - **One Zone**: 
        - One AZ, great for dev. 
        - Backup enabled by default with Infrequent Access. 
        - Over 90% in cost saving

## EBS vs EFS vs Instance Store
- **EBS Volumes**
    - Most of the time one Instance(except multi-attach IO1/IO2)
    - Are locked at the AZ level
    - To migrate EBS volume across AZ you must take a snapshot and restore in the another AZ
- **EFS**
    - Attach to hundreds of instances across AZ at the same time
    - Higher price point than EBS
- **Instance Store**
    - Fisically attached to the EC2 instance.
    - If you lose your instance you will lose the storage