## 1. Oracle Database Server Architecture

* The architecture includes **physical components** (actual files on disk) and **logical components** (instance running in memory).
* It ensures data storage, processing, and access.
---

##  **Oracle Database Architecture Overview**

![image](https://github.com/user-attachments/assets/3f938073-6ac2-46a9-9c00-f0dcc9385c7f)

---

## 2. Physical Structure – Database

These are the files that exist on the storage disk:

* **Datafiles**
  Store the actual data (user data and metadata).

* **Online Redo Log Files**
  Store all changes made to the database to help recover in case of failure.

* **Control Files**
  Contain important metadata about the database like:

  * Database name
  * File locations
  * SCN (System Change Number)

* **Tempfiles**
  Used for temporary operations like sorting or joining large datasets.

---

## 3. Logical Structure – Instance

The instance is the **set of memory and background processes** that manage the database.

* Includes:

  * **Memory Structures** (called SGA - System Global Area)
  * **Background Processes**

---

## 4. Memory Structures

These are areas in memory used for processing and caching data:

* **Shared Pool**
  Caches parsed SQL statements and execution plans to improve performance.

* **Database Buffer Cache**
  Holds recently used data blocks. Reduces disk I/O.

* **Redo Log Buffer**
  Temporarily stores redo entries (changes made) before writing to redo log files.

* **Large Pool**
  Supports large operations like:

  * RMAN backup/restore
  * Parallel queries

* **Java Pool**
  Used when Java code is executed inside the database.

* **Stream Pool**
  Supports Oracle Streams for data replication or messaging.

---

## 5. Background Processes

These are helper processes that automatically manage database tasks:

* **DBWn (Database Writer)**
  Writes dirty (modified) buffers from memory to datafiles.

* **LGWR (Log Writer)**
  Writes redo log buffer contents to online redo log files.

* **SMON (System Monitor)**
  Performs recovery at startup and cleans up temporary segments.

* **PMON (Process Monitor)**
  Cleans up after failed user processes and releases locked resources.

* **CKPT (Checkpoint)**
  Updates datafile headers and control files with checkpoint info.

* **RECO (Recoverer)**
  Automatically resolves failures in distributed transactions.

* **ARCn (Archiver)**
  Copies redo log files to archive logs to support point-in-time recovery.

---

Would you like me to convert this into a README.md file as well?
