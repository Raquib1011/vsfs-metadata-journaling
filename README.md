# VSFS Metadata Journaling Subsystem

[![View PDF](https://img.shields.io/badge/View_Specification-PDF-red?style=for-the-badge&logo=adobeacrobatreader)](./Term%20Project_%20Metadata%20Journaling.pdf)

A crash-consistent metadata journaling module implemented in C for an 85-block Very Simple File System (VSFS) disk image on Linux.

---

## 📐 Disk Layout & Specification

The file system operates on a 4 KB block size across 85 total blocks:

| Component | Block Range | Description |
| :--- | :--- | :--- |
| **Superblock** | Block 0 | File system magic number and global metadata |
| **Journal** | Blocks 1–16 | Append-only transaction logging ring |
| **Inode Bitmap** | Block 17 | Tracks allocated inode slots |
| **Data Bitmap** | Block 18 | Tracks allocated data blocks |
| **Inode Table** | Blocks 19–20 | Holds metadata for file/directory inodes |
| **Data Blocks** | Blocks 21–84 | Stores directory entries and file contents |

---

## 🛠️ Commands & Implementation Details

### 1. `journal create <name>`
* Logs metadata updates without modifying home disk locations.
* Scans bitmaps to allocate a free inode and directory slot.
* Constructs updated metadata blocks in memory.
* Appends `DATA` records followed by a `COMMIT` record into the 16-block append-only journal.

### 2. `journal install`
* Sequentially parses committed journal transactions.
* Replays block images to their target home block numbers.
* Clears (checkpoints) the journal header upon completion.

---

## 📄 Full Specification

For detailed data structures (`struct superblock`, `struct inode`, `struct journal_header`, `struct data_record`) and exact bitmapping specifications, refer to the [Full Specification Document (PDF)](./Term%20Project_%20Metadata%20Journaling.pdf).
