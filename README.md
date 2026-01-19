# Virtual Memory Manager with TLB Simulation

This lab implements a **Virtual Memory Manager** in C that simulates address translation using a **page table**, **Translation Lookaside Buffer (TLB)**, and a **backing store**. It translates logical (virtual) addresses into physical addresses while tracking **page faults** and **TLB hit rates**.

The implementation closely follows concepts taught in operating systems courses, including paging, demand paging, and TLB caching.

---

## 📌 Features

* Logical → Physical address translation
* Page table with valid/invalid bits
* Demand paging using a backing store (`BACKING_STORE.bin`)
* FIFO-based TLB with a maximum of 16 entries
* Page fault handling
* Runtime statistics:

  * Total translated addresses
  * Page fault count and rate
  * TLB hit count and rate

---

## 🧠 System Design Overview

### Address Structure

* **Logical Address**: 16 bits

  * Page Number: 8 bits
  * Offset: 8 bits
* **Page Size**: 256 bytes
* **Number of Pages**: 256
* **Physical Memory Size**: 65,536 bytes

### Components

* **Page Table**

  * 256 entries
  * MSB used as a valid bit
  * Stores frame starting addresses
* **Physical Memory**

  * Simulated as a byte array
* **Backing Store**

  * Binary file containing all pages
* **TLB**

  * FIFO queue
  * Stores (page number, frame address) pairs

---

## 📂 Project Structure

```
.
├── main.c              # Virtual memory manager logic
├── tlb.c               # TLB implementation (FIFO)
├── tlb.h               # TLB definitions and macros
├── BACKING_STORE.bin   # Simulated secondary storage
├── addresses.txt       # Input logical addresses
└── README.md
```

---

## ⚙️ How It Works

1. Read logical addresses from `addresses.txt`
2. Extract page number and offset
3. Check the TLB

   * **Hit** → use cached frame address
   * **Miss** → consult page table
4. If page table entry is invalid

   * Trigger a **page fault**
   * Load page from backing store into physical memory
   * Update page table
5. Update the TLB
6. Generate physical address
7. Read the value from physical memory
8. Print translation details and final statistics

---

## ▶️ Compilation & Execution

### Compile

```bash
gcc main.c tlb.c -o vm_manager
```

### Run

```bash
./vm_manager
```

Make sure `addresses.txt` and `BACKING_STORE.bin` are in the same directory.

---

## 📊 Sample Output

```
Virtual address = 16916, page number = 66, offset = 20, physical address translation 16916, Value = -72
...
Number of translated addresses = 1000
Number of Page Faults = 244
Page Fault rate = 0.244
Number of TLB Hits = 56
TLB Hit rate = 0.056
```

---

## 🧪 Key Metrics Tracked

* **Page Fault Rate**
* **TLB Hit Rate**
* Total translated addresses

These metrics help evaluate the effectiveness of the TLB and paging strategy.

---

## 🛠️ Notes & Limitations

* No page replacement policy (frames grow sequentially)
* Physical memory assumed large enough to hold all pages
* FIFO TLB replacement only
* No concurrency or multi-process support

---

## 📚 Concepts Demonstrated

* Virtual memory
* Paging
* Demand paging
* TLB caching
* Address translation
* Low-level memory management in C

---

## 👩‍💻 Author

Developed as a systems programming / operating systems lab to demonstrate virtual memory management concepts.

---
