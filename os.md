### pagetable
xv6 Memory layout. Increasing address: text (code), data (global var), stack (local vars; grows down), heap (dynamic mem allocate)
A directory in xv6 is an array with 2 bytes for inode # and 14 for name
The satp register in the CPU enables MMU to find the correct page table for the process
The TLB gets flushed during process switches
DACUXWRV

pagetables - demand paging, copy on write, trampoline pages for transition during interrupts
drivers - concurrency issues. "UART" interrupts. devintr handles. interrupts good if unpredictable times; polling is good for high rate

### trap
**during a trap, execute the following unless the trap is a device interrupt and SIE bit is 0**
- clear SIE bit
- copy pc to sepc
- save current mode in SPP bit
- set **scause** to reflect trap cause
- set mode to supervisor
- copy **stvec** to **pc**
- start executing at pc
##### usertraps
uservec (kernel/trampoline.S)
- save 32 user register inside of the TRAPFRAME page, which is 1 below the TRAMPOLINE PAGE, so a constant in all process virtual memory
- changes stvec so that traps in kernel handled by **kernelvec**
- install kernel stack pointer, hartid, pagetable
- jump to usertrap
usertrap (kernel/trap.c)
- determine cause of trap, process it
- either calls systemcall handler, timer interrupt handler, device handler, or exception and kills process
usertrapret (kernel/trap.c)
- turn off interrupts, because we then restore stvec to uservec
- restore trapframe kernelpagetable, kernelstackpointer, usertrap location, hartid
userret (kernel/trampoline.S)
- restore user pagetable
- restore registers from trapframe
- return to user mode
###### kerneltraps
kernelvec (kernel/kernelvec.S)
### riscv
**call {label}** := ra <- pc + 4 ; pc <- (label)
**scause** - supervisor cause; cause of exception, like page fault or syscall. 
- SXLEN-1 -> interrupt (0/1)
- 15 = write page fault
- 13 = load page fault
- 8 = U-mode syscall
- 9 = S-mode syscall
**tp** - where the  current hartid only accessible in machine mode is saved; must be 
**mstatus** - settings for machine mode
**sret** - return to user mode, restore pc from sepc and uses sstatus to determine whether device enabled
**stval** - supervisor trap value -  instruction address or memory address when faulting
**stvec** - supervisor trap-vector base address - 
**sepc** - supervisor exception program counter - instruction address when faulting in supervisor mode. controls where the program returns after trap handling.
**satp** - Supervisor Address Translation and Protection, initialized to kernel pagetable upon boot
**epc** - the return pointer counter
**bss** - the part of a program that hasn't been initialized, like a huge array of 0
in linux, you can do mmap to allocate stuff now, not lazy. in xv6 sbrk is used, which is lazy.
**sstatus** - supervisor status register
- Supervisor Interrupt Enable (SSTATUS_SIE) - whether device interrupts enabled
- SSTATUS_SPP (1=supervisor, 0=user); to what mode **sret** returns
### gdb
**info registers**
**info locals**
**frame**
**command breakpoint number**: type some commands and then end
**break location if condition**
**cond breakpoint number condition**
**next (n)** - step to next line of code, skipping over function calls
**step (s)** - step into function and debug it line-by-line
**finish** - run until current function returns
**continue (c)** - continue running until next breakpoint/event
**until (u)** - run until specific line or end of loop
**stepi (si)** - step thru single machine instruction
**nexti (ni)** step over single machine instruction, skipping function calls
**thread** 1 - switch to thread 1
**thread apply all** bt - applies command to thread
**set scheduler-locking on** - only current thread runs while stepping (on), all threads run while stepping thru code (off), only current thread runs while stepping, but other threads run if use continue (step)
**skip function** function_name - skips running that function
info skip
**skip enable** skip number
**skip delete** skip number
**skip delete** skip number
**watch** expression - pauses program when something changes, like variable or ptr
**rwatch** - read watchpoint - when something is read
**awatch** - access watchpoint - when something is read or written
info watchpoints
delete watchpoint
**where and bt** same thing
**tb** LOCATION - temporary breakpoint (executed once)
**tui enable** - text user interface enable - see both 
**x/1bx** 0x10000005 -> 0x61 (print LSR in UART) - 1 byte, hexadecimal format, at address 0x10...
**x/i** address -> disassemble instruction
**disassemble** address -> disassemble function
**objdump -p user/_ init** - dump out file in ELF format
### UART
UART has 2 fIFO queues for incoming/outcoming bytes. Address 10 million. 
0: RBR/THR (receiver buffer register/transmitter holding register) 1: IER (interrupt enable register), 5: LSR (line status register, bit 0 is if RBR is ready)

sh, read, uart read, sleep (top half)
kernel trap, uart intr (bottom half). this interrupt sticks byte in buffer (cuz process interrupted could be different than process that's reading)
### timer
kernel/start.c allows supervisor-mode access to timer control registers, and asks for first timer interrupt by setting **stimecmp** 
in the future, the **clockintr** in kernel/trap.c sends updates
### scheduling
- there are many processes that can execute user and kernel code, and also scheduler threads to schedule the processes. scheduler separate from process because we don't wanna use the same stack on two different CPUs, and also we want to have multiple CPUs running schedulers at the same time.
- **swtch** saves the callee-saved registers only, because the caller-saved registers can be restored by the caller

### Locks
>bcache.lock - protects allocation of block buffer cache entires
>cons.lock - serializes access to console hardware, avoids intermixed output
>ftable.lock - serializes allocation of a struct file in file table
>itable.lock - protects allocation of in-memory inode entries
>vdisk_lock - serializes access to disk hardware and queue of DMA descriptors
>kmem.lock - serializes allocation of memory
>log.lock - serializes operations on the transaction log
>pipe's pi->lock - serializes operations on each pipe
>pid_lock - serializes increments of next_pid
>proc's p->lock - serializes changes to process's state
>wait_lock - helps wait avoid lost wakeups
>tickslock - serializes operations on the ticks counter
>inode's ip->lock - serializes operations on each inode and its content
>buf's b->lock - serializes operations on each block buffer 

### Filesystem
>data is 512-byte blocks called sectors; **struct buf** stores blocks in-memory.
>- block 0 = boot, 1 = superblock (file system size, number of data blocks, number of inodes, number of blocks in log. filled in by **mkfs**), 2 = log, inodes (multiple inodes per block), bitmap blocks track which data blocks are in use, data blocks (hold file/directory content)
>buffer cache caches disk blocks
>- doubly-linked list. **binit** initializes **NBUF** buffers in **buf** 
>- **bread** **bwrite** work with bufs, and must write to disk if they've changed it. **brelse** releases sleep-lock
>- **bget** finds buffer in linked list with LRU, acquires buffer's sleep-lock, 

### Microkernel
>L4 IPC - sync unbuffered consists of call() and sendrecv()
>- send() recv() buffered
>

### Linux
>dentry (directory entry) is component of Linux Virtual Filesystem layer representing a component of a file path, mapping it to its inode. dentries have pointers to parent and child dentries
>Read-Copy update is synchronization method handling when readers outnumber writers. readers access shared data without locking, writers update via copy and swap, grace period management when no readers are using old data

