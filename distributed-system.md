# Partitioning
 - Horizontal Partitioning
 - Vertical Partitioning

## Horizontal Partitioning
### Algorithms for Horizontal Partitioning
  - Range partitioning
    Range partitioning is a technique where we split a dataset into ranges according to the value of a specific attribute. We then store each range in a separate node. The case we described in the previous lesson—with the alphabetical split—is an example of range partitioning.

    <img width="1886" height="670" alt="image" src="https://github.com/user-attachments/assets/195d1f4d-c20d-4106-a043-8b8ee7f49575" />

  Of course, the system should store and maintain a list of all these ranges and map which node stores a specific range. In this way, the system consults this node map whenever the system receives a request for a specific value (or a range of values) to identify which node (or nodes, respectively) the request should be redirected to.

  - Hash partitioning
  - Consistent hashing
  

## Vertical Partitioning

# CAP theorem
According to the initial statement of the CAP theorem, it is impossible for a distributed data store to provide more than two of the following properties simultaneously: consistency, availability, and partition tolerance.

# symmetric and asymmetric encryption#
Symmetric encryption is the simplest kind of encryption, where there is a single key that can be used both as the encryption and decryption key

Asymmetric encryption uses two separate keys, an encryption key and a decryption key.
- The encryption key is a public key shared with everyone who needs to send encrypted messages to an entity.
- The decryption key is a private key and it is used only by this entity to decrypt incoming messages.

Use cases of encryption
There are two main use cases of encryption, known as encryption in transit and encryption at rest.

# CASE STUDY
## GFS/HDFS () Google File System/ Hadoop Distributed File System

<img width="2046" height="1298" alt="image" src="https://github.com/user-attachments/assets/6498342b-9ed1-4a9c-8e0d-4087b1c575f6" />
# 📄 File Storage Flow in a Distributed File System

When you store a file like `sample.pdf` in a distributed file system, here's how the process typically works:

---

### 1. Chunking the File
- The client library takes the file data and splits it into **fixed-size chunks**.
- Each chunk is assigned an **index** based on its position in the file.
  - Example: `chunk 0` for the first part, `chunk 1` for the second, etc.

---

### 2. Contacting the Manager Node
- The client contacts the **manager node** (or master) to:
  - Get **chunk handles**.
  - Retrieve the **locations of chunk servers** responsible for storing each chunk.

---

### 3. Distributing Chunks to Servers
- The client pushes each chunk’s data to the chunk servers.
- Typically uses **chain replication** to efficiently distribute data to all replicas.

---

### 4. Lease Assignment for Mutations
- The manager grants a **lease** to one chunk server per chunk.
- This chunk server acts as the **primary replica** and serializes write operations.

---

### 5. Reading the File
When the client wants to **read** `sample.pdf`:
- It uses the filename to ask the manager for:
  - The chunk handles.
  - The locations of the corresponding chunk servers.

---

### 6. Retrieving Chunks in Order
- The client requests each chunk from the closest chunk servers.
- It retrieves chunks **in order** starting from `chunk 0`, based on their **byte offsets**.

---

### 7. Reconstructing the File
- The client library **reassembles the chunks** sequentially.
- Reconstructs the **complete file content** for the application.

> This entire process **abstracts away** the chunking from the application, which simply works with the file as a **continuous byte stream**.


