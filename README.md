**Oracle Database Architecture 

This documentation presents an in-depth yet simplified explanation of Oracle Database architecture, excluding Container and Pluggable Database concepts.

---

## 1. **Oracle Database Architecture Overview**

![image](https://github.com/user-attachments/assets/de20e698-7e85-4c8c-ba46-62cbcc665cd1)


Oracle Database is structured into two key layers:

* **Instance**: Memory + background processes
* **Database**: Physical files on disk

These work together to manage data, execute SQL, handle transactions, and ensure recoverability.

---

## 2. **Oracle Instance Architecture**

### 2.1 What is an Instance?

An **Instance** is the combination of:

* Memory structures: Used for caching data, SQL parsing, transactions
* Background processes: Used for writing data, recovery, monitoring

Each time you start an Oracle database, an instance is started in memory.

---

## 3. **Memory Structures**

Memory in an Oracle instance is split into two main parts:

* **System Global Area (SGA)** – Shared by all sessions
* **Program Global Area (PGA)** – Private to each server process

### 3.1 System Global Area (SGA) Components

#### a. **Database Buffer Cache**

* Stores copies of database blocks read from disk
* Improves performance by avoiding frequent I/O
* Blocks here can be dirty (modified) or clean

#### b. **Shared Pool**

* Caches:

  * Parsed SQL and PL/SQL (Library Cache)
  * Data dictionary info (Data Dictionary Cache)
* Reduces CPU usage by reusing execution plans

#### c. **Redo Log Buffer**

* Temporarily holds redo entries (changes made to data)
* LGWR writes it to redo log files
* Crucial for recovery

#### d. **Large Pool** *(Optional)*

* Used for:

  * RMAN backups
  * Shared server processes
  * Parallel queries

#### e. **Java Pool** *(Optional)*

* Memory for Java code execution (when Java is used inside the DB)

#### f. **Streams Pool** *(Optional)*

* Used by Oracle Streams for replication

### 3.2 Program Global Area (PGA)

* Memory allocated per server process
* Not shared
* Used for:

  * Sorting
  * Session-level data
  * Cursors
  * Hash joins

---

## 4. **Oracle Background Processes**

These processes work together to manage memory, perform I/O, and ensure consistency.

### 4.1 Key Background Processes

#### a. **DBWn (Database Writer)**

* Writes modified (dirty) buffers to datafiles on disk
* Multiple DBWn processes may exist (DBW0, DBW1...)

#### b. **LGWR (Log Writer)**

* Writes redo log buffer to redo log files
* Ensures data recovery

#### c. **CKPT (Checkpoint)**

* Signals DBWn to write
* Updates control file and datafile headers with checkpoint info

#### d. **SMON (System Monitor)**

* Recovers after instance crash
* Cleans up temporary segments

#### e. **PMON (Process Monitor)**

* Cleans up failed user processes
* Frees locked resources

#### f. **ARCn (Archiver)**

* Copies redo logs to archive destination (if ARCHIVELOG mode is enabled)

#### g. **RECO (Recoverer)**

* Handles distributed transactions recovery

#### h. **MMON/MMNL (Manageability Monitor/Light)**

* Gathers AWR statistics and sends alerts

---

## 5. **Oracle Database Storage Architecture**

### 5.1 Physical Structure (Disk-level files)

#### a. **Datafiles**

* Contain actual user and system data
* Organized into tablespaces

#### b. **Redo Log Files**

* Record all changes made to data
* Required for crash and instance recovery

#### c. **Control Files**

* Small binary files with metadata:

  * Database name
  * Timestamp
  * Datafile and redo log structure

#### d. **Archive Log Files** *(if enabled)*

* Archived copies of redo log files
* Required for point-in-time recovery

### 5.2 Logical Storage Structure

#### a. **Tablespaces**

* Logical container of data
* Types:

  * SYSTEM: Data dictionary
  * SYSAUX: Auxiliary metadata
  * UNDO: Stores undo records
  * TEMP: For temporary operations
  * USERS: Default user tablespace

#### b. **Segments**

* Space allocated for database objects like tables and indexes

#### c. **Extents**

* A group of contiguous blocks allocated to a segment

#### d. **Blocks**

* Smallest unit of storage (default: 8 KB)
* Data is read/written at block level

---

## 6. **Process Flow - SQL Execution in Oracle**

1. **User sends a SQL query (SELECT/INSERT/UPDATE)**
2. Server Process checks the **Library Cache** for existing parsed SQL
3. If not found, SQL is parsed, and execution plan is generated
4. Required data is fetched from **Datafiles** into **Buffer Cache** (if not already present)
5. If DML, changes are made in memory and redo entries created in **Redo Log Buffer**
6. LGWR writes redo to disk
7. DBWn eventually writes dirty blocks to **Datafiles**

---

## 7. **Diagram: Full Oracle Database Architecture (Simplified)**

```
+-------------------+       +--------------------------+
|    User Process   | <---> |     Server Process       |
+-------------------+       |  (PGA - Private Memory)  |
                            +-----------+--------------+
                                        |
+------------------------------------------------------+
|              System Global Area (SGA)               |
| +------------+  +------------+  +------------------+ |
| | Shared     |  | Buffer     |  | Redo Log Buffer  | |
| | Pool       |  | Cache      |  |                  | |
| +------------+  +------------+  +------------------+ |
| Optional: Large Pool, Java Pool, Streams Pool        |
+------------------------------------------------------+
                                        |
+------------------------------------------------------+
|         Background Processes (DBWn, LGWR, etc.)      |
+------------------------------------------------------+
                                        |
+------------------------------------------------------+
|         Physical Database (Disk Files)               |
| +--------------+  +-------------+  +---------------+ |
| | Datafiles    |  | Redo Logs   |  | Control Files | |
| +--------------+  +-------------+  +---------------+ |
+------------------------------------------------------+
```

---

## 8. **Additional Concepts**

### 8.1 Checkpoint

* Occurs when Oracle ensures all modified blocks are written to disk
* Improves recovery performance

### 8.2 Undo and Temp Segments

* **Undo** stores previous values for rollback
* **Temp** used for sorting and hashing operations

### 8.3 Latches and Locks

* Used to control access to memory and data
* Prevents conflicts in multi-user environments

---

## 9. **Conclusion**

Oracle Database architecture revolves around efficient memory management, disk I/O, and background processes. A strong grasp of these fundamentals helps DBAs and developers understand how SQL executes, data is stored, and how Oracle ensures high availability and recovery.

Advanced topics like Data Guard, ASM, and RAC build on this foundation.
