##Memory Management
////
+ There are different ways to handle memory
  + Direct Access (MS-DOS)
  + Managed Memory
////
####Virtual Memory management benefits

  + Security
  + Transparency
  + Eficiency
  + Multi-programming
  + Flexibility
////
####Virtual Memory is supported through hardware

+ Hardware handles the memory addressing
+ Current hardware uses the Von Neuman memory model
  + Data and Code shares the same memory addressing
////
###Memory Addressing

  + Memory is handled by the Memory Management Unit (MMU)
  + MMU can handle memory in two ways:
    + Real Mode
    + Protected Mode
////
  + Real Mode. Direct access to memory physical address (bootloader)
  + Protected Mode. Three types of address:
     + Physical Address
     + Linear Address
     + Logical Address
////
####Each CPU has implemented a *Memory Arbiter*
####The Memory Arbiter manages the CPU access to RAM and Direct Memory Access (DMA) units
////
####Each memory address type refers to a different way to handling memory

+ Logical Address uses segmentation
+ Linear Address, also called Virtual Address uses Paging

####In order to obtain the physical address, the Operating System must calculate both methods on modern PCs
////
##Segmentation
////
+ Segmentation was introduced since the Intel 80286
+ It was the first method for virtual memory implementation
////
+ A Logical Address used on 32 bits architecture is done using 48 bits
  + The first 16 bits are called a *segment selector*
  + The last 32 bits are called *offset*
////
+ The offset is just an unsigned integer that stores how many memory addresses is a given location from the start of the segment
+ Starting from the Most Significant Bit (MSB), the structure of the Segment Selector is:
  + Bits 15-3 are called index
  + Bit 2 is the Table Descritor
  + Bit 1-0 the permission level
////
+ When a process is being executed by the CPU, three segments are assigned to it:
  + Code Segment, stored on cs
  + Data Segment, stored on ds
  + Stack Segment, stored on ss
+ There are three more segment registers for general use:
  + es
  + fs
  + gs
////
####In order to speed up linear address computation from a logical address, the CPU keeps a *segment descriptor*
////
+ A segment descriptor is needed for each logical address
+ Segment descriptors are stored on a Global Descriptor Table (GDT) or on a Local Descriptor Table (LDT)
////
+ Each CPU has its own GDT, its address is stored in a special register called *gdtr*
+ Each process can have one LDT, when a process is executed, the addres of the LDT is stored in the *ldtr* register
////

////
+ There are four kinds of segment descriptor:
  + Code Segment Descriptor
  + Data Segment Descriptor
  + Task Segment Descriptor
  + Local Table Table Descriptor
////
### Linear Address Calculation Algorithm

+ Check the TL field of the Segment Selector
+ Index times 8 plus the GDT or LDT address
+ Add the offset to the linear address stored on the segment descriptor
////
###Paging
////
+ Paging is another Virtual Memory Model
+ It uses small memory blocks called Pages
////
+ Page characteristics:
  + All linear addresses inside a page are contiguous
  + It's size is 4096 bytes
  + A process can have multiple pages
////
+ The pages are hardware managed by the *Paging Unit*
+ The physical memory (RAM) is also splitted in units of 4KB
+ The physical blocks are called Page Frames
////
+ Each page frame can hold only one Page
+ A page can be seen as a set of data and a page frame as a block of free memory
////
+ The data blocks inside a Page are identified by linear addresses
+ The linear address must be translated to a physical address by the CPU in order to retrieve the stored value
////
+ There are two main ways to handle the linear addresses on the 32 bits x86 architecture
  + Paging: The linear address is splitted in three blocks
  + Extended Paging: The linear address is splitted in two blocks
+ Each address split is known as *level*
////
####Structure of a linear address using the paging method
////
####Table of the paging flags
////
+ Each Page level points to a *Page Table*
+ The page table structure  is related to the Segmentation Tables 
+ Each page table holds the address to either another page table or to a physical address.
////
####Diagram of a Page Table
////
+ The main advantage of Paging over Segmentation is better *swapping*
+ Swapping is the action of moving Pages from RAM memory to another cheaper storage unit.
////
+ Swapping allows the computer to increase the available RAM in a way
+ When a Page is *old enough* and a free page frame is needed, the CPU can allocate the old page in another storage unit like a HDD or a SDD
////
+ How the CPU calculate the *age* of a page?
  + Checks for the Dirty flag 
  + Checks for the Accessed flag
  + Date Bookkeeping
////
+ What happen when a process already in "memory" looks for a page that has been swapped?
  + The CPU will try to find it on RAM
  + Then a *Page Fault* exception is raised
  + Next, the paging unit will look for the missing page on the Swapping file/drive/parttition
  + The paging unit will swap an old Page for the requested one and pass the new address to the CPU
////
+ Extended Paging was introduced on since the Pentium models.
+ It used only two levels instead of three
+ This allows to handle bigger pages of 4 MB each
+ Bigger pages have better performance when bigger files are being handled by a process
////
###The +4GB problem
////
+ Because the RAM memory became cheaper, more and more PCs were being build with more than 4GB of ram
+ However, the way it was, Paging could handle less than 4GB of memory
+ The solution was to *improve* paging with a mechanism called Physical Address Extension (PAE)
////
+ PAE was introduced on the Petium Pro and onwards
+ It extended the Page table entries from 32 to 64 bits
+ The new linear addresses use 36 bits instead of 32
+ In PAE, four levels are used for paging
////
+ On 64 bit architecture, PAE is not needed
+ The levels for paging on 64 bits depends on the architecture
+ x86_64 uses four levels with linear address of 46 bits
////
###Caching
