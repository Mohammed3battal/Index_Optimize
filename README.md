## Script: IndexOptimize_Maintenance.sql

**Description**:
This script runs Ola Hallengren’s `IndexOptimize` stored procedure to automate **index defragmentation** and **statistics updates** for all user databases.

It dynamically decides the best action (reorganize, rebuild, or skip) based on index fragmentation levels. It also updates only **modified statistics**, and logs all actions to a reporting table.

---

### 🔧 Parameters Used:

| Parameter                   | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| `@Databases = 'USER_DATABASES'` | Runs against all user databases (you can change to `'YourDB'`)              |
| `@Indexes = 'ALL_INDEXES'`      | Targets all indexes in each database                                      |
| `@FragmentationLow = NULL`      | Ignores low fragmentation (< 5%)                                          |
| `@FragmentationMedium`         | Reorganize or Rebuild if fragmentation between 5% and 30%                 |
| `@FragmentationHigh`           | Rebuild (online or offline) if fragmentation ≥ 30%                        |
| `@FragmentationLevel1 = 5`     | Medium fragmentation threshold                                            |
| `@FragmentationLevel2 = 30`    | High fragmentation threshold                                              |
| `@UpdateStatistics = 'ALL'`    | Updates all statistics                                                    |
| `@OnlyModifiedStatistics = 'Y'`| Updates only stats that have changed                                      |
| `@LogToTable = 'Y'`            | Logs operations into `CommandLog` table for tracking/reporting            |

---

### 🗂️ What It Does:
- **Reorganizes or rebuilds** indexes based on fragmentation thresholds
- **Skips low-fragmented** indexes (no wasted effort)
- **Updates stats** only when necessary (performance-efficient)
- **Logs all actions** to `dbo.CommandLog` (you can review history)

---

### 📌 Notes:
- Requires [Ola Hallengren’s Maintenance Solution](https://ola.hallengren.com)
- You must have already installed `IndexOptimize` stored procedure
- Use `@Databases = 'YourDatabaseName'` to run it on a specific DB
- Rebuild actions may be **online or offline**, depending on edition & index type

---

###  Use Case:
- Schedule this as a **SQL Agent Job** for nightly or weekly index maintenance
- Combine with backup jobs for full maintenance strategy
- Monitor index health via `CommandLog` table after execution

