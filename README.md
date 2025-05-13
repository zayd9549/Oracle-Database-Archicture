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

---

## ✅ **Oracle Background Processes**

---

### 🔹 **1. DBWR (Database Writer)**

🧠 **Primary Role**:

* Writes **dirty buffers** (modified data blocks) from the **Database Buffer Cache** to **datafiles**.
* Frees up **buffer cache space** for incoming data blocks.

🛠️ **Why Critical?**

* Ensures **durability** by moving data from volatile memory to persistent disk.
* Works with **free buffer management**—if no clean buffer is available, it must flush dirty blocks to make space.

📌 **Triggers**:

* When the buffer cache has too many dirty blocks.
* On **checkpoint**, **tablespace offline**, or **database shutdown**.

📎 **Key Views**: `V$DBFILE`, `V$BUFFER_POOL_STATISTICS`, `V$SYSSTAT`

Great request! Let's expand the **DBWR (Database Writer)** section by adding **buffer types**, **LRU (Least Recently Used) mechanism**, and related **internal memory management logic** — all of which are crucial for a deep understanding of how Oracle handles memory and disk I/O.

---

## 🔹 **1. DBWR (Database Writer)**

🧠 **Primary Role**:

* Writes **dirty buffers** (modified blocks) from the **Database Buffer Cache** (part of SGA) to **datafiles** on disk.
* Frees up buffer cache space by writing old or infrequently used blocks to disk.

---

### ✅ **Types of Buffers in Buffer Cache**

The **Database Buffer Cache** contains **three main types of buffers**:

| Buffer Type        | Description                                                                 |
| ------------------ | --------------------------------------------------------------------------- |
| **Free Buffers**   | Unused buffers ready to be filled with data read from disk.                 |
| **Pinned Buffers** | Buffers currently being accessed by a user session (locked for processing). |
| **Dirty Buffers**  | Buffers that have been modified in memory but **not yet written** to disk.  |

🧾 DBWR is responsible for **flushing dirty buffers** to datafiles and converting them into **free buffers**.

---

### 🧠 **What is the LRU (Least Recently Used) List?**

Oracle uses an **LRU (Least Recently Used) replacement algorithm** to manage buffer cache memory efficiently.

#### 🔄 **How LRU Works**:

* **Most recently accessed blocks** are kept near the **head** of the list.
* **Least recently used** blocks go to the **tail**.
* When the buffer cache is full and a new block is needed, Oracle looks at the **tail** for a **clean (non-dirty)** buffer to reuse.

#### 📌 **But what if buffers at the tail are dirty?**

* Oracle initiates **DBWR** to flush dirty blocks to disk, turning them into **free buffers**.

#### 💡 Internally:

* Oracle actually uses **two sub-lists**:

  * **LRU - Writing Sublist**: Buffers eligible for reuse (includes clean and dirty).
  * **LRU - Touch Sublist**: Recently accessed buffers promoted to avoid being aged out.

---

### 🔄 **DBWR's Triggers (When It Wakes Up)**

| Trigger Event                               | Description                                       |
| ------------------------------------------- | ------------------------------------------------- |
| **Buffer cache has too many dirty buffers** | To prevent shortage of free buffers               |
| **Free buffer shortage**                    | Reclaims space by writing dirty buffers to disk   |
| **Checkpoint**                              | Flushes all dirty buffers to disk for consistency |
| **Tablespace/datafile offline/drop**        | Writes related buffers to disk                    |
| **Database shutdown**                       | Ensures all data is persistent                    |

---

### 🧰 **Performance Considerations**

* **Too many dirty buffers** → Increased pressure on DBWR.
* **Frequent writes** → Higher I/O, but better consistency during recovery.
* **Buffer busy waits** → Happens when no free/pinned buffer is available (DBWR lagging behind).

---

### 📎 **Relevant Views for Monitoring**:

* `V$DB_CACHE_ADVICE` – Buffer cache tuning suggestions
* `V$BUFFER_POOL_STATISTICS` – Hit ratios, dirty buffer stats
* `V$SYSSTAT` – Overall buffer activity (`db block gets`, `consistent gets`)
* `V$WAITSTAT`, `V$SESSION_WAIT` – Identify waits like `buffer busy waits`

---

## 🔹 **2. LGWR (Log Writer)**

🧠 **Primary Role**:

* Writes **redo entries** from the **Redo Log Buffer** (in SGA) to the **online redo log files** on disk.
* Ensures **durability** of committed transactions (crash recovery guarantee).
* Maintains the **ACID (Atomicity, Consistency, Isolation, Durability)** principle — especially **Durability**.

---

### 🧱 **Redo Log Buffer – What LGWR Writes**

* The **Redo Log Buffer** temporarily stores *change vectors* — small pieces of information that describe **data changes** (not the data itself).
* These include DML (INSERT/UPDATE/DELETE), DDL, and internal metadata changes.
* LGWR writes these changes to **online redo log files**.

---

### ⏰ **When Does LGWR Write? (Trigger Events)**

| Trigger Event                      | Description                                                                                           |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **COMMIT / ROLLBACK issued**       | LGWR writes all redo for the committed transaction to redo logs before acknowledging success to user. |
| **Redo Log Buffer one-third full** | Writes automatically to avoid buffer overflow.                                                        |
| **Every 3 seconds (timeout)**      | Default behavior to flush redo if no activity.                                                        |
| **DBWR writes dirty buffers**      | LGWR writes redo first to maintain Write-Ahead Logging (WAL) protocol.                                |
| **Log Switch**                     | LGWR finishes writing to current log and starts the next redo log group.                              |

---

### 🔄 **Write-Ahead Logging (WAL) Principle**

Before **DBWR** can write dirty data blocks to disk, **LGWR must first** flush **redo entries** describing those changes.
This ensures that during crash recovery, Oracle can **reconstruct the changes** using redo logs.

---

### 🧰 **LGWR & Crash Recovery**

* Redo logs written by LGWR are **essential for instance/crash recovery**.
* Upon restart after a crash, **SMON** applies redo from the online redo logs to **bring the DB to a consistent state**.
* Without redo, Oracle can't recover committed transactions.

---

### ⚙️ **Log Groups and Members**

* LGWR writes to **one redo log group at a time**.
* Each group can have **multiple members (multiplexing)** for redundancy.
* In case one member fails, LGWR continues writing to others in the same group.

---

### 🔍 **Performance & Wait Events**

| Issue                      | Description                                                                    |
| -------------------------- | ------------------------------------------------------------------------------ |
| `log file sync` wait       | Happens when sessions wait for LGWR to flush redo (commit latency).            |
| `log file parallel write`  | LGWR waiting for I/O to complete writing redo logs.                            |
| **Frequent commits**       | Can overload LGWR, increasing commit latency.                                  |
| **Slow I/O for redo logs** | Directly impacts overall DB performance. Use fast disks or SSDs for redo logs. |

---

### 📎 **Relevant Views for Monitoring**

* `V$LOG` – Status of redo log groups
* `V$LOGFILE` – Redo log file names and paths
* `V$SYSSTAT` – Redo size, writes, commits
* `V$SESSION_WAIT` – Look for `log file sync` or `log file parallel write`

---

### 💡 Best Practices

* Use **multiple redo log members** per group (multiplexing).
* Place redo logs on **fast storage** (preferably SSD or separate disk).
* Avoid **too frequent commits** in application logic.

---

## 🔹 **3. SMON (System Monitor)**

🧠 **Primary Role**:
SMON is responsible for **instance recovery**, **temporary space cleanup**, and **space management** tasks.
It ensures that the database starts in a consistent state after an **abnormal shutdown or crash**.

---

### 🔁 **SMON's Core Responsibilities**

| Function                      | Description                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Crash Recovery**            | Applies committed changes from redo logs and rolls back uncommitted ones using the **redo and undo**. |
| **Temporary Segment Cleanup** | Frees space used by **temporary segments** that were not deallocated (e.g., failed sort operations).  |
| **Coalescing Free Space**     | Merges adjacent free extents in **dictionary-managed tablespaces** to reduce fragmentation.           |
| **Shrinking Undo Segments**   | Reclaims undo space no longer in use.                                                                 |
| **Instance/Startup Recovery** | Automatically runs if Oracle detects a crash upon startup.                                            |

---

### 🩺 **Crash Recovery (Instance Recovery)**

When Oracle detects that the **last shutdown was not clean** (e.g., power failure, kill -9, crash), SMON initiates **instance recovery**, consisting of two main steps:

#### 1️⃣ Redo (Roll Forward)

* Reads **online redo logs**.
* **Re-applies committed changes** that were not yet written to datafiles.

#### 2️⃣ Undo (Roll Back)

* Uses undo information to **rollback**:

  * Incomplete/uncommitted transactions.
  * Transactions from users who were mid-operation during crash.

📌 **SMON uses:**

* Redo log files (written by LGWR)
* Undo segments (managed via undo tablespace)

---

### ⚙️ **SMON & Temporary Segments**

* Temporary segments can be created during operations like:

  * `ORDER BY`, `GROUP BY`, `SORT`, `CREATE INDEX`, etc.
* If a session ends unexpectedly, SMON removes these **orphaned temp segments** automatically.

---

### 📏 **SMON & Free Space Coalescing**

* In **dictionary-managed tablespaces**, SMON:

  * Periodically merges adjacent free extents.
  * Helps reduce **space fragmentation**.
* In **locally managed tablespaces (LMTs)**, this is no longer needed (bitmap-based management).

---

### 🕵️‍♂️ **Monitoring SMON Activity**

| View         | Purpose                                                                     |
| ------------ | --------------------------------------------------------------------------- |
| `V$SESSION`  | SMON will show up as a background process with `PROGRAM='SMON'`.            |
| `V$PROCESS`  | Tracks the OS process linked to SMON.                                       |
| `V$UNDOSTAT` | Undo segment usage info (relevant during recovery).                         |
| `ALERT.LOG`  | Will log crash recovery steps (roll forward, roll back, recovery duration). |

---

### 🛑 **What Happens During a Crash? (Simplified Flow)**

1. Crash happens (e.g., power outage).
2. Upon next startup:

   * Oracle checks **SCN in control file vs datafiles**.
   * Detects mismatch → triggers SMON.
3. SMON:

   * Applies redo (from logs) to redo committed changes.
   * Rolls back any uncommitted transactions.
4. Database opens **clean and consistent**.

---

### 🔐 **SMON is Critical**

* Without SMON, Oracle **cannot perform instance recovery**.
* It is started automatically and **must always be running** in any healthy Oracle instance.

---

### 💡 Best Practices & Notes

* You don’t manually control SMON—it is automatic and essential.
* If you observe **long startup times after a crash**, check:

  * Size of redo logs.
  * Number of uncommitted transactions.
  * Disk I/O performance (affects redo read & undo application).
* Always check `alert.log` after instance recovery for details.

---

## 🔹 **4. PMON (Process Monitor)**

🧠 **Primary Role**:
PMON is responsible for **detecting failed user and server processes**, **cleaning up resources** they held, and **restoring availability** of those resources to the system.

---

### 🧹 **PMON's Core Responsibilities**

| Function                       | Description                                                                             |
| ------------------------------ | --------------------------------------------------------------------------------------- |
| **Process Cleanup**            | Cleans up after user or background process failure (e.g., kills, crashes).              |
| **Session Resource Release**   | Frees up memory, rollback segments, locks, cursors, and temp segments.                  |
| **Listener Registration**      | Registers instance information with the Oracle Listener (Dynamic Service Registration). |
| **Restart Failed Dispatchers** | In shared server mode, restarts failed dispatchers and servers.                         |

---

### ⚠️ **When Does PMON Get Triggered?**

PMON is **event-driven**, not time-based:

* Triggered **only** when a **process failure or crash** is detected.
* Common causes:

  * `kill -9` on a client session.
  * Network disconnection.
  * Application crashes without proper logoff.

---

### 🔄 **Steps Performed by PMON on Session Crash**

Let’s say a user’s session suddenly terminates mid-transaction. PMON will:

1. **Detect** the abnormal process termination.
2. **Free PGA memory** allocated to that user session.
3. **Release locks** (DML/DDL) held by the session.
4. **Mark any active transactions** for rollback by SMON.
5. **Deallocate temp segments** used by the session.
6. **Close cursors** or connections left open.
7. **Restore session slots** for reuse.

---

### 📡 **PMON & Listener Communication**

PMON performs **Dynamic Registration** with the Oracle **Listener**, especially important when:

* `SERVICE_NAMES`, `INSTANCE_NAME`, `HOST`, `PORT` info needs to be broadcasted.
* Makes instance available in `TNSNAMES.ORA` without manual updates.

💡 If PMON fails to register (e.g., listener not yet up), it will **retry periodically**.

---

### 🛠️ **Shared Server Architecture Role**

If Oracle is configured for **Shared Server Mode**, PMON also:

* **Restarts** failed dispatcher processes.
* **Monitors** shared server pool health.

---

### 🔍 **Monitoring PMON**

| View               | Use                                                          |
| ------------------ | ------------------------------------------------------------ |
| `V$SESSION`        | PMON shows as a background process.                          |
| `V$PROCESS`        | See OS PID tied to PMON.                                     |
| `V$RESOURCE_LIMIT` | Indirectly helps monitor freed session slots.                |
| `ALERT.LOG`        | Logs PMON-related startup/shutdown info and listener issues. |

---

### 💡 **Important Notes**

* PMON works closely with SMON:

  * **PMON detects** a dead session.
  * **SMON rolls back** the incomplete transaction (if needed).

* PMON is **not responsible** for rolling back uncommitted work — that’s SMON’s job.

* Dynamic registration eliminates the need for manual entries in `listener.ora`.

---

### 📌 **Real-World Use Case**

> **Scenario**: A developer kills a session during a `DELETE` statement on a large table.

🔄 Behind the scenes:

* PMON detects session failure.
* Releases locks on that table.
* Cleans up PGA memory and cursors.
* Marks transaction for rollback.
* SMON handles undo if transaction was active.

---

### ❗ **Without PMON...**

* Sessions would leave **orphaned locks**, **unreleased memory**, **hanging temp segments**, and more.
* Over time, this would lead to memory pressure, lock contention, and unstable system behavior.
Here’s a comprehensive, in-depth breakdown of:

---

## 🔹 **5. CKPT (Checkpoint Process)**

🧠 **Primary Role**:
CKPT is responsible for **signaling a database checkpoint**, which ensures that all modified (dirty) buffers in the memory are written to disk and that checkpoint metadata is updated in **datafile headers** and **control files**.

---

### 🚩 **What is a Checkpoint?**

A **checkpoint** is a critical event where Oracle synchronizes the in-memory data (SGA) with the on-disk data (datafiles).
It guarantees **consistency** and helps minimize **recovery time** in case of failure.

---

### ✅ **Functions of CKPT**

| Function                        | Description                                                                                        |
| ------------------------------- | -------------------------------------------------------------------------------------------------- |
| 🧠 **Signals DBWR**             | Tells the **DBWR** process to write dirty buffers from **buffer cache** to **datafiles**.          |
| 📝 **Updates Datafile Headers** | Writes the current **System Change Number (SCN)** and checkpoint info into each datafile's header. |
| 📄 **Updates Control File**     | Writes checkpoint SCN into the control file to maintain database structure and recovery sync.      |
| 📊 **Coordinates With LGWR**    | Works alongside LGWR to ensure redo logs and datafiles are in sync.                                |

---

### 🔁 **When Are Checkpoints Triggered?**

| Trigger Type               | Description                                                |
| -------------------------- | ---------------------------------------------------------- |
| 🕒 **Timed Checkpoints**   | Based on `LOG_CHECKPOINT_TIMEOUT` (e.g., every N seconds). |
| 🔢 **Log Switch-Based**    | After every redo log switch (`LOG_CHECKPOINT_INTERVAL`).   |
| 🛠️ **Manual Checkpoint**  | Issued manually with `ALTER SYSTEM CHECKPOINT;`.           |
| 📦 **Shutdown Checkpoint** | Triggered during `SHUTDOWN IMMEDIATE` or `NORMAL`.         |
| 🛑 **Recovery Checkpoint** | Used during crash recovery or instance recovery.           |

---

### 📦 **Why Are Checkpoints Important?**

| Benefit                      | Explanation                                                               |
| ---------------------------- | ------------------------------------------------------------------------- |
| ✅ **Faster Recovery**        | Reduces the number of redo log entries Oracle must apply during recovery. |
| ✅ **Data Consistency**       | Ensures changes in memory are made persistent in the datafiles.           |
| ✅ **Efficient Buffer Cache** | Frees up dirty buffers after flushing them to disk.                       |

---

### 🧩 **How CKPT Interacts With Other Components**

| Component                | Interaction                                 |
| ------------------------ | ------------------------------------------- |
| **DBWR**                 | Signals DBWR to write dirty buffers.        |
| **Control File**         | Updates SCN and checkpoint info.            |
| **Datafile Header**      | Writes checkpoint SCN.                      |
| **Redo Logs (via LGWR)** | Coordinates SCN sync between data and redo. |

---

### 🔍 **Monitoring CKPT**

| View                | Description                                       |
| ------------------- | ------------------------------------------------- |
| `V$DATAFILE_HEADER` | Check SCN and timestamp of last checkpoint.       |
| `V$DATABASE`        | See checkpoint progress & SCN.                    |
| `V$SYSSTAT`         | Look for checkpoint statistics.                   |
| `ALERT.LOG`         | Logs checkpoint start and completion with reason. |

---

### 🧠 **CKPT vs DBWR vs LGWR**

| Process  | Responsibility                                          |
| -------- | ------------------------------------------------------- |
| **CKPT** | Orchestrates the checkpoint and updates metadata (SCN). |
| **DBWR** | Writes actual dirty blocks to disk.                     |
| **LGWR** | Writes redo entries (change records) to redo log files. |

---

### 🔧 **Checkpoint Tuning Parameters**

| Parameter                 | Purpose                                                         |
| ------------------------- | --------------------------------------------------------------- |
| `FAST_START_MTTR_TARGET`  | Specifies target mean time to recover (auto-tunes checkpoints). |
| `LOG_CHECKPOINT_INTERVAL` | Checkpoint every N redo blocks written.                         |
| `LOG_CHECKPOINT_TIMEOUT`  | Checkpoint every N seconds.                                     |

---

### 📌 **Real-World Use Case**

> **Scenario**: You issue `SHUTDOWN IMMEDIATE`.

* CKPT is triggered.
* It signals DBWR to flush dirty buffers.
* CKPT updates datafile headers and control files with the SCN.
* Ensures a clean, recoverable state for the next startup.

---

### ⚠️ **If CKPT Fails or Is Delayed...**

* Longer crash recovery times.
* More redo entries to process.
* Higher chance of data inconsistencies in memory vs disk.


---

Here is a deep-dive explanation of:

---

## 🔹 **6. RECO (Recoverer Process)**

🧠 **Primary Role**:
The **RECO (Recoverer)** background process **automatically resolves failures in distributed transactions** (two-phase commits). It ensures **atomicity and consistency** across databases in distributed systems.

---

### 🌐 **What is a Distributed Transaction?**

A **distributed transaction** involves multiple databases (or instances), typically using **database links (DBLINKs)**. Oracle must ensure **all participants either commit or roll back** to preserve data integrity.

Example:

```sql
INSERT INTO orders@remote_db ...;
```

---

### 🔁 **Two-Phase Commit (2PC) Protocol**

RECO works in the **second phase** of this protocol to ensure success or rollback:

| Phase                | Action                                                                     |
| -------------------- | -------------------------------------------------------------------------- |
| **Phase 1: Prepare** | All databases prepare to commit and respond if they’re ready.              |
| **Phase 2: Commit**  | Coordinator sends COMMIT if all responded OK. Else, sends ROLLBACK.        |
| **If Crash Happens** | And coordinator fails mid-process, **RECO** resolves when system restarts. |

---

### ✅ **Functions of RECO**

| Function                              | Description                                                                                                                                             |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🔄 **Resolves In-Doubt Transactions** | Automatically connects to other databases and tries to **commit or rollback** distributed transactions that were left in an uncertain (in-doubt) state. |
| 🧩 **Maintains Consistency**          | Ensures **ACID** compliance across all involved systems.                                                                                                |
| 📡 **Periodically Scans**             | Continuously checks the `DBA_2PC_PENDING` table for transactions to recover.                                                                            |
| 🛠️ **Works With ORA-24756 Errors**   | Detects failed distributed transactions that need action.                                                                                               |

---

### 📝 **Common View: `DBA_2PC_PENDING`**

Stores metadata of in-doubt distributed transactions:

```sql
SELECT local_tran_id, global_tran_id, state FROM dba_2pc_pending;
```

| Column           | Meaning                                                             |
| ---------------- | ------------------------------------------------------------------- |
| `STATE`          | Can be `collecting`, `prepared`, `committed`, `forced commit`, etc. |
| `LOCAL_TRAN_ID`  | Internal ID for tracking.                                           |
| `GLOBAL_TRAN_ID` | Coordinated across databases.                                       |

---

### ⚠️ **States of In-Doubt Transactions**

| State                         | Meaning                                  |
| ----------------------------- | ---------------------------------------- |
| `prepared`                    | Transaction is ready but needs decision. |
| `collecting`                  | Waiting for response from other nodes.   |
| `committed` / `forced commit` | Finalized.                               |
| `rollback forced`             | User-forced rollback.                    |

---

### 🧠 **RECO in Action – Real World Example**

**Scenario**:
A transaction spans databases **DB1** and **DB2**.
Just before commit, **DB1 crashes**.
Now DB2 is in a “prepared” state but doesn't know whether to commit or roll back.

✅ When DB1 restarts, RECO:

1. Identifies the in-doubt transaction.
2. Connects to DB2 using the transaction ID.
3. Coordinates the **commit or rollback**.
4. Removes the entry from `DBA_2PC_PENDING`.

---

### 🔐 **Security Considerations**

* RECO requires **network access** to contact remote DBs.
* Should be monitored in secure environments to avoid unwanted resolution.

---

### 🧩 **Associated Parameters**

| Parameter                  | Purpose                                 |
| -------------------------- | --------------------------------------- |
| `DISTRIBUTED_TRANSACTIONS` | Max number of distributed transactions. |
| `DISTRIBUTED_LOCK_TIMEOUT` | How long Oracle waits for remote locks. |

---

### 🔍 **Monitoring & Troubleshooting**

| View                   | Use                                              |
| ---------------------- | ------------------------------------------------ |
| `DBA_2PC_PENDING`      | List of unresolved distributed transactions.     |
| `V$GLOBAL_TRANSACTION` | Runtime information on distributed transactions. |
| `ALERT.LOG`            | Logs when RECO takes recovery action.            |

---

### 🔧 **Manual Intervention (If Needed)**

Use `FORCE COMMIT` or `FORCE ROLLBACK` if RECO can't resolve:

```sql
COMMIT FORCE 'transaction_id';
ROLLBACK FORCE 'transaction_id';
```

---

### 📌 **Best Practices**

* Ensure **RECO is running** in any DB using DBLINKs.
* Periodically monitor `DBA_2PC_PENDING`.
* Use **timeout settings** to avoid hanging remote locks.

---

Here is the detailed explanation for:

---

## 🔹 **7. ARCn (Archiver Process)**

🧠 **Primary Role**:
The **ARCn (Archiver)** background process **copies full (filled) redo log files** from the online redo log to **archive log destinations** — only **when the database is in ARCHIVELOG mode**.

It plays a critical role in **data recovery**, **Data Guard**, and **backup strategies**.

---

### 🔁 **Why Archive?**

Online redo logs are **cyclical and reused**. If they are overwritten before a backup, **you lose recovery ability**.
**ARCn prevents this by saving full redo logs** as archived logs before reuse.

---

### 🛠️ **Functions of ARCn**

| Function                         | Description                                                                                          |
| -------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 📂 **Archives Filled Redo Logs** | Copies redo log groups once they are full and **LGWR switches** to a new group.                      |
| 🔄 **Supports Recovery**         | Enables **point-in-time recovery (PITR)** and **media recovery**.                                    |
| 🌐 **Ships Logs Remotely**       | In Oracle Data Guard, ARCn or related processes (like `LNSn`) **ship logs to the standby database**. |
| 📊 **Maintains Log History**     | Keeps record of sequence numbers and log switches in `V$ARCHIVED_LOG` and `V$LOG_HISTORY`.           |

---

### ⚙️ **How ARCn Works with LGWR**

1. LGWR fills redo log group.
2. Redo log switch occurs (automatically or via `ALTER SYSTEM SWITCH LOGFILE`).
3. ARCn **archives** the full redo log file to destinations like:

   * File System
   * ASM
   * FRA (Fast Recovery Area)
   * Remote Standby (for Data Guard)

---

### 🧩 **Associated Parameters**

| Parameter                      | Description                                       |
| ------------------------------ | ------------------------------------------------- |
| `LOG_ARCHIVE_DEST_n`           | Destination(s) to archive logs (up to 31).        |
| `LOG_ARCHIVE_FORMAT`           | Naming convention for archived logs.              |
| `LOG_ARCHIVE_MAX_PROCESSES`    | Number of ARCn processes (default is 4).          |
| `LOG_ARCHIVE_MIN_SUCCEED_DEST` | Minimum number of destinations that must succeed. |

---

### 🧠 **Important Views**

| View                    | Description                                       |
| ----------------------- | ------------------------------------------------- |
| `V$ARCHIVED_LOG`        | Lists archived logs (status, name, applied, etc.) |
| `V$LOG_HISTORY`         | Shows historical log switch info                  |
| `V$ARCHIVE_DEST_STATUS` | Monitors archive destination status               |
| `V$ARCHIVE_PROCESSES`   | Shows ARCn process status                         |

---

### 🧪 **Example Archived Log File Names**

* `arch_0001_124.arc`
* `1_34567_1122334455.dbf`

Naming depends on `LOG_ARCHIVE_FORMAT`, e.g.:

```bash
LOG_ARCHIVE_FORMAT = 'arch_%t_%s_%r.arc'
```

Where:

* `%t` – Thread #
* `%s` – Sequence #
* `%r` – Resetlogs ID

---

### 🛡️ **In Oracle Data Guard**

ARCn works alongside:

* `LNSn` (Log Network Server)
* `RFS` (Remote File Server)

In **Maximum Performance mode**, **ARCn ships archived logs** to the standby database.

---

### 🔍 **Monitoring Log Gaps (DG)**

```sql
SELECT sequence#, applied FROM v$archived_log WHERE destination = 'STANDBY';
```

---

### ⚠️ **Common Issues**

| Problem                     | Cause                                                           |
| --------------------------- | --------------------------------------------------------------- |
| `ORA-00257: archiver error` | Archive destination (FRA or disk) full                          |
| Archive lag                 | ARCn delay or network/IO bottleneck                             |
| Archive not happening       | ARCHIVELOG mode not enabled or `LOG_ARCHIVE_DEST` misconfigured |

---

### 🧠 **Best Practices**

* Place archive logs on a **separate disk group or mount** to avoid I/O contention.
* Regularly **backup and delete** old archive logs.
* Enable **FRA monitoring** if using Fast Recovery Area.
* Monitor archive status using:

```sql
SELECT dest_id, status, destination, error FROM v$archive_dest_status;
```
---

