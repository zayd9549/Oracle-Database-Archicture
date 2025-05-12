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
Perfect — here’s the **refined version** of ✅ **1. Datafiles** with the following corrections:

* A **stronger, more polished definition**
* Extended points in **Purpose**
* 📎 **Extension** added after 🧪 Examples
* Format aligned with your expectations (clean, direct, no fluff)

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
*A control file is a small binary file that records the structure and metadata of the database. It is essential for database startup and recovery.*

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

🔍 **Query Example**:

```sql
SELECT name FROM v$controlfile;
```

---

## ✅ **4. Tempfiles**

📘 **Definition**:
*Tempfiles provide temporary disk storage for operations like sorting, hashing, and global temporary table storage. They are not backed up and are cleared after shutdown.*

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

🔍 **Query Example**:

```sql
SELECT file_name, tablespace_name, bytes/1024/1024 AS size_mb FROM dba_temp_files;
```

---

## ✅ **5. Archivelog Files**

📘 **Definition**:
*Archived logs are offline copies of redo logs generated in ARCHIVELOG mode. They are essential for point-in-time recovery and disaster recovery.*

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

  ```sql
  DELETE ARCHIVELOG ALL COMPLETED BEFORE 'SYSDATE-7';
  ```

