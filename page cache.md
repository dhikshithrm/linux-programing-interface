a page cache, sometimes also called disk cache, is a transparent cache for the pages originating from a secondary storage device such as a hard disk drive (HDD) of solid-state drive (SSD). The operating system keeps a cache in otherwise unused portions of the main memory (RAM), resulting in quicker access to the contents of cached pages and overall performance improvements. A page cache is implemented in [[Kernel]]s with [[paging]] memory management, and is mostly transparent to applications.

Usually, all physical memory(RAM) not directly allocated to applications is used by the operating system for the page cache. Since the memory would otherwise be idle and is easily reclaimed when applications request it, there is generally no associated performance penalty and the operating system might even report such memory as "free" or "available".

Memory mappings serve a variety of purposes, including initialization of a
process’s text segment from the corresponding segment of an executable file,
allocation of new (zero-filled) memory, file I/O (memory mapped I/O), and inter-
process communication (via a shared mapping).