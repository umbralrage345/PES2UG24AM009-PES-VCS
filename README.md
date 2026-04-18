# PES Version Control System (PES-VCS)

## Overview

PES-VCS is a lightweight Git-inspired version control system implemented in **C** as part of the Operating Systems project. It demonstrates how real version control systems manage data internally using:

* Content-addressable object storage
* SHA-256 hashing
* Blob / Tree / Commit objects
* Staging area (Index)
* Branch references
* Commit history traversal

This project recreates core Git concepts from scratch for learning filesystem structures, object storage, indexing, and repository management.

---

# Features Implemented

## Phase 1 – Object Storage

Implemented low-level object database.

### Added:

* `object_write()`
* `object_read()`

### Supports:

* Blob objects
* Tree objects
* Commit objects

### Functionality:

* Hash object content using SHA-256
* Store objects in sharded directories:

```text
.pes/objects/ab/cdef1234...
```

* Read back stored objects
* Prevent duplicate writes if object already exists
* Atomic writes using temporary files + rename

---

## Phase 2 – Tree Objects

Implemented tree structures representing snapshots of directories/files.

### Added:

* Tree entry structure
* Tree serialization
* Tree parsing
* Deterministic ordering
* Tree object creation

### Supports:

* File metadata
* File names
* Object hashes
* Snapshot representation of project state

---

## Phase 3 – Index / Staging Area

Implemented staging area similar to Git index.

### Added:

* `index_load()`
* `index_save()`
* `index_add()`
* `index_remove()`
* `index_find()`
* `index_status()`

### Index File:

```text
.pes/index
```

### Stores:

* File mode
* Blob hash
* File size
* Modification time
* Relative path

### Commands Working:

```bash
./pes add file1.txt
./pes status
```

---

## Phase 4 – Commits and History

Implemented commit creation and history traversal.

### Added:

* `commit_create()`
* `commit_load()`
* `commit_walk()`

### Commit Contains:

* Tree hash
* Parent commit hash
* Author
* Timestamp
* Message

### Commands Working:

```bash
./pes commit -m "Initial commit"
./pes log
```

---

# Internal Repository Structure

After initialization:

```text
.pes/
├── HEAD
├── index
├── objects/
│   ├── xx/
│   └── yy/
└── refs/
    └── heads/
        └── main
```

---

# Commands Supported

## Initialize Repository

```bash
./pes init
```

Creates:

* `.pes/`
* object store
* refs
* HEAD

---

## Add Files

```bash
./pes add file1.txt file2.txt
```

Stages files into index.

---

## Check Status

```bash
./pes status
```

Shows:

* Staged files
* Modified files
* Deleted files
* Untracked files

---

## Create Commit

```bash
./pes commit -m "message"
```

Creates tree + commit object and updates branch reference.

---

## Show Log

```bash
./pes log
```

Displays commit history.

---

# Build Instructions

## Linux / WSL

```bash
make clean
make
```

---

## Run Tests

### Phase 1

```bash
make test_objects
./test_objects
```

### Phase 2

```bash
make test_tree
./test_tree
```

### Full Integration

```bash
make test-integration
```

---

# Important Fixes Made During Development

## Object Layer

* Improved read validation
* Improved write error handling
* Fixed temporary file writes

## Tree Layer

* Added missing headers
* Removed unnecessary index dependency
* Improved tree object generation

## Index Layer

* Fixed segmentation faults
* Corrected save/load parsing
* Corrected status display
* Added text-format index output

## Commit Layer

* Parent linking support
* HEAD updates
* Branch reference updates
* Log traversal

---

# Example Workflow

```bash
./pes init

echo hello > file1.txt
./pes add file1.txt
./pes commit -m "first commit"

echo world >> file1.txt
./pes add file1.txt
./pes commit -m "second commit"

./pes log
```

---

# Example Log Output

```text
commit a1b2c3d4...
Author: PES User <pes@localhost>
Date: 1776472020

    second commit

commit e5f6g7h8...

    first commit
```

---

# Concepts Demonstrated

This project demonstrates:

* Filesystem metadata
* Hashing
* Snapshot storage
* Persistent data structures
* Atomic file updates
* Branch references
* Commit chains
* Working tree tracking

---

# Technologies Used

* C Programming
* GCC
* Linux / WSL
* OpenSSL SHA-256 (`-lcrypto`)
* Makefiles
* POSIX filesystem APIs

---

# Learning Outcome

By building PES-VCS manually, we understand how Git internally manages:

* Objects
* Trees
* Commits
* Branches
* HEAD
* Index
* Repository history

---

# Future Improvements

Possible future additions:

* Branch creation
* Checkout
* Merge
* Diff engine
* Garbage collection
* Tags
* Remote push/pull
* Reflog

---

# Authors

Developed as Operating Systems Lab Project.

---

# Final Status

All required phases completed:

* Phase 1 ✅
* Phase 2 ✅
* Phase 3 ✅
* Phase 4 ✅
* Phase 5 Theory ✅
* Phase 6 Theory ✅

Project successfully built and tested.

---
