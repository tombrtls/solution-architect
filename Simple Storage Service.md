# Storage Service, Glacier and CloudFront

## Simple Storage Service
### Storage Classes
There are a couple of different Storage Classes, each has it's own purpose and costs associated with it.

1. Standard
2. Standard - Infrequently Accessed
3. One Zone - Infrequently Accessed
4. Reduced Redundency

### Version Control
You can enable version control for your bucket (but you won't be able to turn it of afterwards). This will keep track of versions of your file. 

By simply uploading another file with the same name, it'll consider it a new version. Through the console you can access all the version. Even deleting a file creates a new version of it (delete marker), which can be removed.

### Cross Region Replication
When creating multiple buckets (in different regeions), you can setup a replication rule. 

We can replicate all content or use a prefix to select folders. You can specify another bucket to replicate it to (which requires versioning). while setting this up you can also change the storage class of the replicated objects (to Standard, Standard - IA, Reduced Redundancy)

You have to have a IAM role for data replication which can be created for you on demand or you can select on.

Objects that are already located in the bucket won't be replicated, you'll have to do this manually.

Any updates will automatically be replicated to the destination S3 Bucket. Deleting individual versions and deleting delete markers are the only things that won't be replicated.

### Life Cycle Management
With Life Cycle Rules you can setup rules that will automatically move objects to certain Storage Classes after a certain period of time. You can setup different rules for the latest version of your objects versus previous versions of them. For example: Transtition to Standard-IA after 30 days or Transition to Amazon Glacier after 60 days.

### Security
By default buckets are private, you need to manually make them publicly accessible (on bucket or object level). You can secure your bucket using `Bucket Policies` or `Access Control Lists`.

### Encryption
- In Transit
  - SSL/TLS
- At Rest
  - Server Side Encryption
    - S3 Managed Keys - SSE-S3
    - AWS Key Management Service, Managed Keys - SSE-KMS
      - Allows for Audit Trails
      - Manage your own Encryption Keys
    - Service Side Encryption with Customer Provided Keys - SSE-C
      - Manage your own keys
  - Client Side Encryption
    - Manage your own keys/encryption methods and apply before uploading

### Storage Gateway
Storage Gateway allows you to integrate AWS S3 into your On-Premise Data Centres. It's a downloadable VM image that you can install and configure. After setting up, you can manage it from the AWS Web Console.

There are multiple types:
- File Gateway (NFS)
  - Used to store flat files such as Word Documents or PDFs.
  - Application Service -> NFS -> Storage Gateway -> (Direct Connect | Internet | VPC) -> S3 -> S3-IA -> Glacier
- Volume Gateway (iSCI) Block based storage
  - Virtual Hard Disk which can be backed up as `point-in-time` snapshots which can be stored as EBS snapshots
  - Stored Volumes
    - Store your entire volume locally. This data is backed up to S3
  - Cached Volumes
    - Uses S3 as the primary data storage. Most recent read data is cached on premise.
- Tape Gateway (VTL)
  - Used for backups of off Tapes which can be synced with `S3` and you can use `Life Cycle Rules` to migrate to `Glacier`

## CloudFront
CloudFront allows you to setup and CDN for your S3 Bucket, EC2 Instance, Elastic Load Balancer or Route 53. It gives you a cache layer across multiple geographical locations (Edge LocationS). This will reduce the latency for any cached objects. This means that the first person to request a certain object won't benefit from this, but subsequent people will. This

CloudFront doesn't just provide a caching layer for read operations, it also allows uploads to be sped up. It'll upload files to the Edge Locations before being uploaded to the actual bucket. 

Objects are cached for the duration of their Time To Live (TTL). You can also clear objects manually (which will cost money).

### Web Application Firewall
When setting up CloudFront you can also assign a Web Application Firewall (WAF) that can block traffic.