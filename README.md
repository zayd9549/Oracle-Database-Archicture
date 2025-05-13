## ✅ **RDBMS**

**RDBMS** stands for **Relational Database Management System**.

* It's software that helps you **store**, **manage**, and **retrieve** data **in tables** (rows and columns).
* Think of it like **Excel**: Each table is like a worksheet, with rows and columns.
* In an RDBMS, **data in one table** can be **linked (related)** to data in another table using **keys**.
* Examples: **Oracle**, **MySQL**, **SQL Server**, **PostgreSQL**, **IBM Db2**.

---

## 🔍 Detailed Explanation 

### 1. 📘 **Definition**

An **RDBMS (Relational Database Management System)** is a type of **DBMS (Database Management System)** that stores data in a **structured format using rows and columns**. It is based on **E.F. Codd's Relational Model** (introduced in 1970).

---

### 2. 📊 **Data Organization**

* **Tables :** Data is stored in **tables**, where each table represents one entity (e.g., `EMPLOYEES`, `DEPARTMENTS`).
* **Rows :** Each row represents a **record**.
* **Columns :** Each column represents a **field** (like `EMP_ID`, `NAME`, `SALARY`).

---

### 3. 🔑 **Keys and Relationships**

* **Primary Key:** Uniquely identifies each row in a table.
* **Foreign Key:** Creates a **relationship** between two tables (e.g., `DEPT_ID` in `EMPLOYEES` refers to `DEPT_ID` in `DEPARTMENTS`).

---

### 4. 🔐 **Features of RDBMS**

| Feature                 | Description                                                              |
| ----------------------- | ------------------------------------------------------------------------ |
| **Data Integrity**      | Ensures accuracy and consistency using constraints like PK, FK, NOT NULL |
| **Normalization**       | Process of organizing data to avoid redundancy                           |
| **SQL Support**         | Standard language (Structured Query Language) for querying the database  |
| **Multi-user access**   | Multiple users can access the database at the same time                  |
| **Security**            | Access control using **roles, users, privileges**                        |
| **Backup & Recovery**   | Mechanisms to protect and restore data                                   |
| **Transaction support** | Ensures ACID properties: Atomicity, Consistency, Isolation, Durability   |

---

### 5. 🔄 **ACID Properties of Transactions**

| Property        | Meaning                                                     |
| --------------- | ----------------------------------------------------------- |
| **Atomicity**   | All steps of a transaction are completed or none            |
| **Consistency** | Data remains in a consistent state after a transaction      |
| **Isolation**   | Transactions are isolated from each other (no interference) |
| **Durability**  | Once a transaction is committed, the changes are permanent  |

---

### 6. 🧠 **Why RDBMS is Important for Oracle DBA**

As an Oracle DBA, you will:

* Create and manage **schemas** (collections of tables and objects)
* Ensure **data integrity** across relationships
* Optimize **SQL queries** and **indexes**
* Manage **backups, recovery**, and **user access**
* Monitor **performance and space** usage

Oracle is one of the most powerful and feature-rich **RDBMS platforms** available, used widely in enterprises for mission-critical applications.

---

## ✅ **1. Oracle Block**

#### 🔍 **Definition:**

An **Oracle Block** (also called a **data block**) is the **smallest unit of storage** in the Oracle Database. It is where actual table data (rows) is stored.

---

#### 🔸 **Purpose:**

* To **store actual rows** of table data.
* It is the **unit of I/O**—Oracle reads and writes data in blocks.
* Forms the **foundation of logical storage** in the database.

---

#### 🔸 **Details:**

* The size of a block is defined by the `DB_BLOCK_SIZE` parameter at database creation.
* Default size: **8 KB**, but can be 2K, 4K, 8K, 16K, or 32K.
* All blocks belong to **extents**, which are part of **segments**.

---

#### 🔸 **Internal Structure:**

| Section             | Description                                                              |
| ------------------- | ------------------------------------------------------------------------ |
| **Block Header**    | Metadata about the block (SCN, block type, transaction slots, etc.)      |
| **Table Directory** | Info about all tables sharing this block (relevant for clustered tables) |
| **Row Directory**   | Pointers to the actual rows                                              |
| **Free Space**      | Unused space for new data                                                |
| **Row Data**        | The actual data rows stored in this block                                |

---

#### 🔸 **Block Behavior:**

* **Row Chaining**: Occurs when a row is too large to fit into one block—spans multiple blocks.
* **Row Migration**: Happens when a row is updated and no longer fits in its original block—migrated to another block.

---

#### 🔸 **Block-Level Optimizations:**

* Uses **freelists** or **bitmap freelists** for managing available space.
* Blocks can be managed in **locally managed tablespaces** with **automatic segment space management (ASSM)**.

---

#### 🧠 **Real-World Analogy:**

A **block** is like a **page in a notebook**.

* The page contains lines (rows) where data is written.
* Every page has a header (page number, title).
* Pages are grouped into chapters (segments).

---

#### 🔸 **Example:**

If your `DB_BLOCK_SIZE` is 8K, each data block can store approx:

* \~100 rows (if each row is \~80 bytes)
* Fewer rows if rows are wide (many columns or large data types)

---

## ✅ **2. Oracle Segment**

#### 🔍 **Definition:**

A **Segment** is a set of contiguous Oracle database blocks that are allocated for a specific database object. Each segment stores data for a specific structure like a table, index, or rollback.

---

#### 🔸 **Purpose:**

* A segment is responsible for **storing all the data** associated with a database object (e.g., rows of a table, index entries).
* A **logical storage unit** that groups database blocks for a specific object.

---

#### 🔸 **Types of Segments:**

1. **Table Segment**: Stores all the rows for a specific table.
2. **Index Segment**: Stores all the entries of an index.
3. **Undo Segment**: Stores undo information to maintain transaction consistency.
4. **Temporary Segment**: Used for storing intermediate data during complex operations (e.g., sorting, joins).
5. **Rollback Segment**: Used for transaction rollbacks and maintaining consistency.

---

#### 🔸 **Details:**

* **Extent Allocation**: Segments are made up of extents, and extents are made up of blocks.
* **Extent Growth**: When a segment needs more space, it acquires additional extents.
* **Segment Header**: Contains metadata, such as the number of extents allocated and segment's status.

---

#### 🔸 **Segment Components:**

| Component          | Description                                                   |
| ------------------ | ------------------------------------------------------------- |
| **Segment Header** | Stores the segment’s metadata (allocated extents, status)     |
| **Data Blocks**    | Stores the actual data (rows for tables, entries for indexes) |
| **Freelist**       | Tracks free space within the segment for efficient allocation |

---

#### 🔸 **Optimizations:**

* **Locally Managed Tablespaces (LMTs)** use **Automatic Segment Space Management (ASSM)**, which helps manage the space within segments more efficiently.
* Segments can be **shrinked** or **migrated** based on data movement and optimization needs.

---

#### 🧠 **Real-World Analogy:**

A **segment** is like a **chapter** in a book:

* The chapter is made up of several pages (blocks).
* All the content for a specific topic (e.g., table data) is within this chapter.
* A chapter may grow or shrink, but it remains dedicated to a specific topic.

---

#### 🔸 **Example:**

* A table with 1000 rows may initially use one segment.
* If the table grows and needs more space, Oracle will allocate more extents to the segment until it grows to the required size.

---

## ✅ **3. Oracle Tablespace**

#### 🔍 **Definition:**

A **Tablespace** is a logical storage container in an Oracle Database that groups related logical structures, such as segments, for physical storage management.

---

#### 🔸 **Purpose:**

* A **tablespace** defines how data is physically stored in the database.
* It is a **container** for storing segments and helps in managing storage allocation across the database.

---

#### 🔸 **Details:**

* A tablespace is mapped to **one or more physical data files** on disk.
* Each tablespace contains **segments**, and each segment is made up of **extents**.
* It acts as an abstraction layer between the physical storage (data files) and the logical database structures (tables, indexes).

---

#### 🔸 **Types of Tablespaces:**

1. **Permanent Tablespaces**:

   * Stores permanent user data (e.g., tables, indexes).
   * Can be **system**, **undo**, **temp**, or **users** tablespaces.
2. **Temporary Tablespaces**:

   * Used for sorting, joining, and temporary data processing.
   * Do not store permanent user data.
3. **Undo Tablespaces**:

   * Stores undo information (used for transaction rollbacks).
4. **System Tablespace**:

   * Contains Oracle's system objects, including data dictionary tables.

---

#### 🔸 **Tablespace Components:**

| Component     | Description                                                           |
| ------------- | --------------------------------------------------------------------- |
| **Datafiles** | Physical files on disk that store the actual data for the tablespace  |
| **Segments**  | Logical storage units (e.g., tables, indexes) within the tablespace   |
| **Extents**   | Groups of contiguous blocks within a segment, allocated to store data |
| **Blocks**    | Smallest unit of storage (actual rows of data) stored within extents  |

---

#### 🔸 **Tablespace Management:**

1. **Dictionary-Managed Tablespaces (DMT)**:

   * Space management is handled through the data dictionary.
2. **Locally Managed Tablespaces (LMT)**:

   * Space management is handled using bitmaps stored within the tablespace, offering better performance.

---

#### 🔸 **Tablespace Features:**

* **Autoextend**: Tablespace data files can be set to automatically extend when space is required.
* **Quota**: User quotas can be set on tablespaces, limiting the amount of space each user can use.
* **Offline/Online**: Tablespaces can be taken offline for maintenance or storage reorganization and brought online afterward.

---

#### 🧠 **Real-World Analogy:**

A **tablespace** is like a **filing cabinet**:

* The cabinet holds multiple **drawers** (segments).
* Each drawer is assigned a label (e.g., for tables or indexes) and holds **files** (data blocks).
* The cabinet organizes all your files in a structured way, making it easy to locate and manage data.

---

#### 🔸 **Example:**

* A **users** tablespace might contain all user tables and indexes, while a **temp** tablespace is used to handle intermediate data during complex queries.
* A tablespace like **undo** will store all transaction-related data for rollback, while **system** stores Oracle's internal data dictionary.

---

## 🔷 **Oracle Database Server Architecture**

---

![image](https://github.com/user-attachments/assets/2ce8a270-d306-4d64-9fd6-854d314d858e)


### 🗄 **PHYSICAL STRUCTURE**

**(Under "Database")**
These components reside on **disk** and are **persistent**, meaning they exist even when the database instance is stopped.

---

## ✅ **1. Datafiles**

📘 **Definition**:
*Datafiles are physical operating system files that permanently store all structured data in an Oracle database — including user objects (tables, indexes), internal data (undo, LOBs), and system metadata. Without datafiles, a database cannot function.*

📊 **Purpose / Usage**:

* Store all **persistent user and system data**
* Maintain **logical storage** for tablespaces
* Enable **transaction management**, **undo**, and **query access**
* Integral to **startup, backup, and recovery** operations

🛠️ **Belong To**:
*Tablespaces such as* `SYSTEM`, `SYSAUX`, `USERS`, `TEMP`, `UNDO`

⚙️ **Characteristics**:

* Can be set to **AUTOEXTEND**
* Structured as **blocks → extents → segments**
* Must be **available** for the DB to open
* Can be backed up using **RMAN**

🧪 **Examples**:

* `system01.dbf`
* `users01.dbf`
* `undotbs01.dbf`

📎 **Extension**: `.dbf`

🔍 **V\$ Views**:

* `DBA_DATA_FILES` – Lists all permanent datafiles
* `V$DATAFILE` – Runtime status of datafiles

📎 **Query Example**:

```sql
SELECT file_name, tablespace_name, autoextensible
FROM dba_data_files;
```

---

## ✅ **2. Online Redolog Files**

📘 **Definition**:
*Online redo logs are files that record all changes made to the database as they happen. They are crucial for crash recovery and instance recovery.*

📊 **Purpose / Usage**:

* Allow **instance/crash recovery**
* Capture every **DML/DDL operation**
* Used by **LGWR** to write redo records

🔁 **Mode**: Work in a **circular fashion** (filled → switch → reuse)

<img width="516" alt="image" src="https://github.com/user-attachments/assets/2d168bfc-3116-4959-80b3-ee9a3940d303" />


⚙️ **Characteristics**:

* Each **log group** has one or more **members**
* Automatically reused in sequence
* Written **sequentially** by the **LGWR** background process
* Should be **multiplexed** to prevent single point of failure

🧪 **Examples**:

* `redo01.log`
* `redo02.log`

📎 **Extension**: `.log`

🔍 **Query Example**:

```sql
SELECT group#, sequence#, status, archived FROM v$log;
```
---

## ✅ **3. Controlfiles**

📘 **Definition**:
*A control file is a crucial binary file used by Oracle to track the physical structure of the database. It contains metadata such as database name, log history, file locations, and backup information — and is required for the database to start and function properly.*

📊 **Purpose / Usage**:

* Maintain metadata about the database:

  * DB name & ID
  * SCNs (System Change Numbers)
  * Redo logs & datafiles
  * RMAN backup and archive log history

⚙️ **Characteristics**:

* Required at **every startup**
* Should be **multiplexed** across different disks
* Updated every time a structural change occurs (e.g., add datafile/logfile)

🧪 **Examples**:

* `control01.ctl`
* `control02.ctl`

📎 **Extension**: `.ctl`

🔍 **V\$ Views**:

* `V$CONTROLFILE` – Lists controlfile names and paths

📎 **Query Example**:

```sql
SELECT name FROM v$controlfile;
```
---

## ✅ **4. Tempfiles**

📘 **Definition**:
*Tempfiles are special datafiles used for temporary storage during SQL operations that require sorting, joining, or working with large datasets. They belong to the TEMP tablespace, are recreated as needed, and are not used to store permanent data.*

📊 **Purpose / Usage**:

* Support operations like:

  * `ORDER BY`, `GROUP BY`
  * Hash joins
  * Bitmap index creation
  * Global temporary tables

🛠️ **Associated With**: TEMP tablespace

⚙️ **Characteristics**:

* Not persistent across shutdowns
* Not backed up (can be **recreated**)
* Only used for **temporary** workspace, not permanent data

🧪 **Examples**:

* `temp01.dbf`
* `temp_undotbs1.dbf`

📎 **Extension**: `.dbf`

🔍 **V\$ Views**:

* `DBA_TEMP_FILES` – Shows tempfiles and their sizes
* `V$TEMPFILE` – Runtime information for tempfiles

📎 **Query Example**:

```sql
SELECT file_name, tablespace_name, bytes/1024/1024 AS size_mb
FROM dba_temp_files;
```
---

## ✅ **5. Archivelog Files**

📘 **Definition**:
*Archivelog files are offline copies of full redo logs created when the database runs in ARCHIVELOG mode. They are critical for database recovery beyond the last backup, enabling point-in-time recovery and supporting standby database synchronization.*

📊 **Purpose / Usage**:

* **Point-in-time recovery** (PITR)
* Oracle **Data Guard** log shipping
* Disaster recovery scenarios
* Backups with **RMAN**

🛠️ **Generated When**: DB is in **ARCHIVELOG** mode
🔄 **Created By**: ARCn (Archiver) background process

⚙️ **Characteristics**:

* Not reused like redo logs
* Stored in file system or ASM
* Can be compressed, backed up, or shipped remotely
* Controlled by `LOG_ARCHIVE_DEST_n` and `LOG_ARCHIVE_FORMAT`

🧪 **Examples**:

* `arch_0001_123.arc`
* `1_34567_1122334455.dbf`

📎 **Extension**: `.arc` or `.dbf` (depending on configuration)

🔍 **V\$ Views**:

* `V$ARCHIVED_LOG` – Lists all archived logs and their statuses
* `V$LOG_HISTORY` – History of redo log switches and archiving

📎 **Query Example**:

```sql
SELECT name, thread#, sequence#, applied
FROM v$archived_log
ORDER BY sequence#;
```

🧠 **Best Practice**:

* Periodically delete old archive logs to free FRA space:
0
```sql
DELETE ARCHIVELOG ALL COMPLETED BEFORE 'SYSDATE-7';
```
---

## ✅ **6. Parameter Files (PFILE / SPFILE)**

📘 **Definition**:
*Parameter files (PFILE/SPFILE) store initialization parameters that define how the Oracle instance is configured during startup. They control memory allocation, file locations, and various performance and security settings.*

📊 **Purpose / Usage**:

* Start the **Oracle instance** with essential configuration settings
* Define memory structures (SGA, PGA), process limits, file paths
* Change instance behavior dynamically (with **SPFILE**)
* Required at **startup**

🛠️ **Types**:

* **PFILE (init.ora)** – Text-based, manually editable
* **SPFILE (spfile.ora)** – Binary, persistent, supports dynamic changes

⚙️ **Characteristics**:

* Only **one** is used at a time (PFILE or SPFILE)
* SPFILE supports **ALTER SYSTEM ... SCOPE** commands
* SPFILE is preferred for **automation** and **dynamic tuning**
* Stored in `$ORACLE_HOME/dbs` or ASM (depending on configuration)

🧪 **Examples**:

* `initORCL.ora`
* `spfileORCL.ora`

📎 **Extension**: `.ora`

🔍 **V\$ Views**:

* `V$SPPARAMETER` – Shows values stored in SPFILE
* `V$PARAMETER` – Shows current active parameter values

📎 **Query Example**:

```sql
SELECT name, value, isspecified
FROM v$spparameter
WHERE name LIKE 'sga%';
```

🧠 **Best Practice**:

* Use **SPFILE** in production for better manageability
* Always **back up** the parameter files before changes
* Keep a **PFILE** copy handy for emergency startup situations

---

## ✅ **7. Password Files**

📘 **Definition**:
*A password file is a separate operating system-level file that stores authentication credentials for Oracle users who need to perform administrative tasks remotely (like SYSDBA). It enables privileged connections without OS authentication.*

📊 **Purpose / Usage**:

* Enable **remote** SYSDBA/SYSOPER access using `sqlplus sys as sysdba`
* Allow **non-OS users** to perform administrative tasks
* Used during **RMAN**, **Data Guard**, **startup/shutdown**, and **Oracle Net** connections

🛠️ **Managed By**: `orapwd` utility

⚙️ **Characteristics**:

* Stored under `$ORACLE_HOME/dbs` or ASM
* Can be **shared** across RAC nodes (in ASM or on shared file system)
* Contains entries for users with `SYSDBA`, `SYSOPER`, `SYSBACKUP`, etc.
* Case-sensitive (if `PASSWORD_CASE_SENSITIVE=TRUE`)
* Can be used in **EXCLUSIVE** or **SHARED** mode

🧪 **Examples**:

* `orapwORCL`

📎 **Extension**:

* No fixed extension (typically `orapw<SID>`)

🔍 **V\$ Views**:

* `V$PWFILE_USERS` – Lists users and their SYSDBA/SYSOPER privileges

📎 **Query Example**:

```sql
SELECT username, sysdba, sysoper, sysbackup
FROM v$pwfile_users;
```

🧠 **Best Practice**:

* Restrict access to the password file to avoid unauthorized privilege escalation
* Use **ORAPWD** tool to manage changes
* Periodically review users with high privileges
---

## ✅ **8. Alert Log**

📘 **Definition**:
*The alert log is a special diagnostic log file that records critical database events and messages. It is the first place DBAs check for errors, startup/shutdown info, and background process messages.*

📊 **Purpose / Usage**:

* Track **startup**, **shutdown**, **log switches**, **errors**, and **recovery actions**
* Record **ORA- errors**, block corruption, and background process alerts
* Critical for **troubleshooting**, **tuning**, and **auditing**

🛠️ **Generated By**: Oracle background processes (e.g., PMON, SMON, LGWR)

⚙️ **Characteristics**:

* Plain text file, **continuously appended**
* Located under `alert_<SID>.log`
* Found in:

  * Non-ADR: `$ORACLE_BASE/admin/<db_name>/bdump`
  * ADR-enabled: `$ORACLE_BASE/diag/rdbms/<db_name>/<SID>/trace/`
* Used heavily in **OEM alerts**, **monitoring**, and **custom scripts**

🧪 **Examples**:

* `alert_ORCL.log`
* `alert_PRODDB.log`

📎 **Extension**: `.log`

🔍 **Query Example**:

```sql
SELECT value 
FROM v$diag_info 
WHERE name = 'Diag Trace';
```

Then navigate to `alert_<SID>.log` in that path.

🧠 **Best Practice**:

* Regularly **monitor and archive** alert logs
* Integrate with tools like **OEM**, **Nagios**, or **Prometheus**
* Watch for frequent ORA-xxxx or repetitive messages indicating deeper issues

---

## ✅ **9. Trace Files**

📘 **Definition**:
*Trace files are diagnostic files generated by Oracle when background or user processes encounter errors or need to log detailed execution paths. They are essential for performance tuning and troubleshooting.*

📊 **Purpose / Usage**:

* Capture **errors**, **execution plans**, and **internal operations**
* Used by Oracle Support for **debugging** and **incident diagnosis**
* Helpful for analyzing **performance bottlenecks** and **wait events**

🛠️ **Generated By**:

* **Background processes** (e.g., DBWR, LGWR, SMON)
* **User sessions** (e.g., for SQL tracing, 10046 events)

⚙️ **Characteristics**:

* Stored in the **trace directory**
* One file per process or session
* Location varies:

  * **ADR enabled**: `$ORACLE_BASE/diag/rdbms/<db_name>/<SID>/trace/`
  * **Non-ADR**: `$ORACLE_HOME/rdbms/log/` or `$ORACLE_BASE/admin/<SID>/udump/`

🧪 **Examples**:

* `ora_12345.trc` (user session trace)
* `smon_7890.trc` (background process trace)

📎 **Extension**: `.trc`

🔍 **Query Example (trace directory location)**:

```sql
SELECT value 
FROM v$diag_info 
WHERE name = 'Diag Trace';
```

🧠 **Best Practice**:

* Use `ALTER SESSION SET SQL_TRACE = TRUE;` to generate specific session traces

* Use **TKPROF** to interpret trace file content:

  ```bash
  tkprof ora_12345.trc output.txt sys=no
  ```

* Regularly **purge old trace files** to save space

* Automate collection during errors via **ADRCI incidents**

## 🧠 **LOGICAL STRUCTURE**

**(Under "Instance")**
These are **memory structures and processes** that exist only **while the instance is running**.

📘 **Definition**:
*Logical structures in Oracle represent how data is organized and managed in memory and storage, independent of the physical layout.*


## 🔹 **Memory Components**

---

## ✅ **1. System Global Area (SGA)**

📘 **Definition**:
*The System Global Area (SGA) is a shared memory region that contains data and control information for one Oracle database instance. It is used by all server and background processes.*

📊 **Purpose / Usage**:

* Enable **shared access** to critical data across user sessions
* Reduce **disk I/O** through caching
* Support **SQL parsing**, **execution**, **logging**, and **data access**

⚙️ **Characteristics**:

* Allocated at **instance startup**
* Size governed by parameters like `SGA_TARGET`, `SGA_MAX_SIZE`
* Can use **Automatic Shared Memory Management (ASMM)**
* Shared across all connected sessions

🧠 **Key Components**:

### 🔷 **a. Database Buffer Cache**

* Stores copies of data blocks read from datafiles
* Helps avoid frequent disk I/O by reusing data in memory
* Organized in **dirty**, **pinned**, and **free** lists
* Modified blocks are eventually written to disk by **DBWn**

📎 V\$ Views:

* `V$BH`, `V$BUFFER_POOL_STATISTICS`

---

### 🔷 **b. Shared Pool**

* Caches **SQL statements**, **PL/SQL procedures**, and **data dictionary** information
* Reduces parsing overhead and improves response time
* Divided into:

  * **Library Cache**: Stores parsed SQL/PLSQL
  * **Data Dictionary Cache**: Stores object definitions, privileges

#### 📚 Library Cache (within Shared Pool)

* **Stores**:

  * Parsed SQL statements
  * Execution plans
  * PL/SQL blocks (procedures, functions)
* **Benefits**:

  * Eliminates hard parsing
  * Improves CPU and memory efficiency
* **Operations**:

  * On repeated query, checks cache → soft parse if found
* **Common Issues**:

  * **SQL not shared** due to literals instead of bind variables
  * **ORA-04031** if cache space is exhausted

---

#### 🗂 Data Dictionary Cache (Row Cache)

* **Purpose**: Caches **object metadata** and privileges.
* **Contents**:

  * Table definitions
  * User roles
  * Column datatypes
  * Privilege metadata
* **Faster than** querying data dictionary base tables.
* **Supports**: Parsing and validation of SQL.

📎 V\$ Views:

* `V$LIBRARYCACHE`, `V$ROWCACHE`

---

### 🔷 **c. Redo Log Buffer**

* Temporarily holds **redo entries** (change vectors) before they’re written to **online redo logs**
* Written by the **LGWR** process during:

  * Commit
  * Log switch
  * Every 3 seconds or when 1/3 full

📎 V\$ Views:

* `V$LOG`, `V$LOGFILE`, `V$LOG_HISTORY`

---

### 🔷 **d. Large Pool**

* Optional component used for:

  * **RMAN** backups
  * **Parallel queries**
  * **Shared Server** UGA
* Prevents fragmentation of the shared pool

📎 Parameter: `LARGE_POOL_SIZE`

---

### 🔷 **e. Java Pool**

* Memory area used for **Java execution**, especially when running Java stored procedures or EJBs inside the DB

📎 Parameter: `JAVA_POOL_SIZE`

---

### 🔷 **f. Streams Pool**

* Used by **Oracle Streams**, **Advanced Queuing**, and **GoldenGate**
* Stores queued messages, staging areas, etc.

📎 Parameter: `STREAMS_POOL_SIZE`

---

🔍 **Query Example**:

```sql
SELECT component, current_size/1024/1024 AS size_mb 
FROM v$sga_dynamic_components;
```

📎 **View** total SGA usage:

```sql
SELECT name, value/1024/1024 AS size_mb 
FROM v$sga;
```

---

## ✅ **2. Program Global Area (PGA)**

📘 **Definition**:
*PGA is a non-shared memory region containing data and control information for a server process. Each user gets their own PGA, and it is not accessible by others.*

📊 **Purpose / Usage**:

* Manage **session-specific operations** like sorting, joins, and buffers
* Support **session memory**, **private SQL areas**, **work areas**

🧠 **Includes**:

* **Sort area**
* **Session memory**
* **Hash join area**
* **Bitmap merge area**
* **Cursor state info**

⚙️ **Characteristics**:

* Allocated **per session**
* Controlled via `PGA_AGGREGATE_TARGET` or `PGA_AGGREGATE_LIMIT`
* Used heavily by **OLAP** and **parallel query** operations

🔍 **Query Example**:

```sql
SELECT name, round(value/1024/1024, 2) AS size_mb 
FROM v$pgastat 
WHERE name IN ('total PGA allocated', 'maximum PGA allocated');
```

---



📎 **View-level Summary**:

* `V$SGA`: Total shared memory usage
* `V$PGASTAT`: Runtime stats for PGA usage
* `V$PROCESS_MEMORY`: Memory by process
* `V$MEMORY_TARGET_ADVICE`: Recommendations


