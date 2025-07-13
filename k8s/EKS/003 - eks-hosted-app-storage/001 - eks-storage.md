`Amazon Elastic Block Store (EBS), Amazon Elastic File System (EFS), and Amazon FSx (including Lustre and NetApp ONTAP), S3, `

CSI : Container Storage Interface

# Persistent Storage

## EBS 

EBS : Can be attached to worker nodes (EC2s) ; EBS volumes are ideal for databases, persistent application data, and workloads requiring high performance block storage

Note: 
* Config of vol can be changed dynamically, even if vol is attached to the instance (elastic)
* DB related, high throughput (High IOPS, Throughput Speed)
	* Quickly accessible and Long persistent data


CSI: Dynamic provisioning of block storage from within the cluster

## EFS

EFS: Fully managed NFS file system that can be shared across multiple EKS worker nodes and pods. EFS is well-suited for shared file access, content management systems, and applications that require a shared file system.

CSI: Enables the use of EFS as persistent vol within the cluster

## Amazon FSx

FSx: FSx for Lustre and FSx for NetApp ONTAP

FSx for Lustre: FSx for Lustre offers high-performance, scalable file storage, especially for compute-intensive workloads and high-performance computing (HPC) applications.

FSx for NetApp ONTAP: FSx for NetApp ONTAP provides a fully managed, enterprise-grade file storage service with features like snapshots and replication.

## S3

S3: Amazon S3, object storage, can be used with EKS through a CSI driver that allows access to S3 objects as if they were files. This is useful for storing large amounts of unstructured data and for applications that can work with object storage semantics

# Ephemeral Storage

## K8s Vol

Ephemeral volumes that exist only for the lifetime of a pod. These can be useful for temporary storage needs, like caching or storing intermediate files.

## Instance store vol

Instance store volumes are temporary storage directly attached to the EC2 instance. While providing high performance, data on instance store volumes is lost when the instance is terminated or restarted. Therefore, they are not suitable for persistent storage needs.

# Dynamic Vol Provisioning

EKS supports dynamic volume provisioning, meaning that storage volumes can be automatically created when a pod needs storage. This is managed by Kubernetes through storage classes and persistent volume claims (PVCs)

EKS Auto Mode automatically creates storage classes using EBS volumes