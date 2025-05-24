# Network File System (NFS) Implementation

This project is a C-based distributed **Network File System (NFS)**, designed to facilitate client access to centralized storage over a network. It supports essential file operations like reading, writing, creating, deleting, and streaming. The system is composed of multiple clients, a central naming server, and distributed storage servers. Together, they manage requests and perform operations efficiently and reliably.

---

## Project Overview

The goal of this system is to simulate a scalable and fault-tolerant NFS environment. Clients can perform a variety of file operations by communicating with a central Naming Server, which directs them to appropriate Storage Servers. The system emphasizes performance, concurrency, and reliability through features like asynchronous writes and replication.

---

## System Components

### 1. Clients

Clients act as the access point to the NFS. They initiate file operations like **create**, **delete**, **read**, **write**, and **stream**. These operations are routed through the **Naming Server**, which determines the correct **Storage Server** responsible for a file.

### 2. Naming Server (NM)

The Naming Server is the core directory service of the system. It maps requested file paths to corresponding Storage Servers and maintains the current state of the file system, including all paths and directories. It updates dynamically as changes occur in the file structure.

### 3. Storage Servers (SS)

These servers manage the actual file data. Each Storage Server handles a segment of the file system, maintaining files and directories. They perform persistent file operations and provide streaming services. The NM handles registration and communication routing for these servers.

---

## Key Features

### File Operations

- **Creating Files or Folders**: Clients can send requests to create new files or directories. The Naming Server (NM) assigns the task to an available Storage Server.
- **Deleting Files or Folders**: Clients can request deletion of existing files or folders. The NM forwards the request to the appropriate SS for deletion.
- **Reading Files**: Clients can access files stored on an SS, which streams the content directly to the client.
- **Writing to Files**: Clients can write data to files with support for both asynchronous and synchronous modes of operation.
- **Audio File Streaming**: Clients can stream audio files in real-time from the Storage Server without downloading them.

### Synchronous vs. Asynchronous Writing

- **Synchronous Mode**: Suitable for critical operations, it ensures data is written before proceeding.
- **Asynchronous Mode**: Enables clients to initiate file writes and continue with other tasks without waiting, ideal for large data transfers.

### Support for Multiple Clients

- **Simultaneous Reads**: Multiple clients can read from the same file concurrently.
- **Exclusive Writes**: Only one client at a time can write to a file, maintaining data integrity.

### Robust Error Handling

- Detailed and descriptive error messages like:
  - `File Not Found`
  - `File in Use`
  - `Server Failure`
- These help in quickly diagnosing and resolving issues.

### Optimized Searching and Caching

- **Efficient Lookup Structures**: The system uses **hashmaps** and **tries** for quick file path resolution.
- **Caching Strategy**: Implements **Least Recently Used (LRU)** caching to improve response times for frequently accessed files.

### Backup, Redundancy, and Recovery

- **File Replication**: Each file is stored on multiple servers to prevent data loss.
- **Async Duplication**: When writing, the file is also asynchronously duplicated to backup servers.
- **Server Sync**: In the event of server downtime, returning servers automatically synchronize their data.

---

## Installation Guide

To set up and run the NFS system:

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/nfs.git
   cd nfs
   ```

2. Build all components:
   ```bash
   ./script.sh
   ```

3. (Optional) Clean up binaries:
   ```bash
   ./clean.sh
   ```

4. Start the Naming Server:
   ```bash
   ./namingServer <port>
   ```

5. Start one or more Storage Servers:
   ```bash
   ./storageServer <naming_server_ip> <naming_server_port> <ss_port> <port_for_clients> <base_path>]
   ```

6. Start a Client:
   ```bash
   ./client <naming_server_ip> <naming_server_port>
   ```

---

## Usage Examples

- **Create a file:**  
  ```
  CREATE <directory_path> <filename> -f
  ```
- **Create a directory:**  
  ```
  CREATE <parent_directory_path> <dirname> -d
  ```
- **Read a file:**  
  ```
  READ <file_path>
  ```
- **Write to a file:**  
  ```
  WRITE <file_path> <data>
  ```
- **Delete a file:**  
  ```
  DELETE <directory_path> <filename>
  ```
- **Copy a file or directory:**  
  ```
  COPY <source_path> <destination_path>
  ```
- **Stream an audio file:**  
  ```
  STREAM <file_path>
  ```
- **Get file info (size and permissions):**  
  ```
  GET <file_path>
  ```
- **Exit the client:**  
  ```
  EXIT
  ```
---

## Directory Structure

- `CS/` - Client source code
- `NS/` - Naming Server source code
- `SS/` - Storage Server source code
- `LRU/` - LRU cache implementation
- `async/` - Asynchronous write simulation
- `fold1/`, `fold2/`, `testdir/`, `new/` - Sample data and test directories

---

## Configuration

- Most configuration is done via command-line arguments when starting servers and clients.
- You can specify storage paths and ports as needed.

---

## Logging

- All server and client actions are logged in `log.txt`.
- Each entry includes the operation, IP, and port for traceability.

---

## Troubleshooting

- **Port already in use:**  
  Make sure previous instances are stopped or use a different port.
- **File not found:**  
  Check if the file path is correct and accessible by the storage server.
- so on...

---

## Contributing

Contributions are welcome! Please fork the repository and submit pull requests. For major changes, open an issue first to discuss what you would like to change.

---
