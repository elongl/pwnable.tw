## Information
- The notes are stored on the `.bss` section as `void* notes[5]` (max note count - 5).
- Each note points to a note struct that consists of a printer function, and the content itself which is allocated on the heap.
- Deleting a note frees up the memory of the note, but doesn't decrement the note counter, nor nulls the pointers on the notes array.
- Value that is returned from `atoi` is signed, and then passed to `malloc` which is unsigned.
- Printing a note after it was deleted attempts to call the printer function on the content, both of which had already been freed, causing a segfault (use-after-free).
- The same note can be freed twice (segfault) because the pointer isn't being nulled (use-after-free).
- I can use one of libc's strings that end with `"sh"` to spawn the shell.


## Leads
- If the content's `malloc` call would return a pointer to a note, it would be possible to overwrite the printer function.
- Overwriting `puts@got` to `system` so that when it's about to print the note,  
it'll call `system(note_content)` rather than `puts(note_content)`.


## Goals
- [x] Overwrite the printer function.
- [ ] Leak libc base address.


## Findings
- If two notes are created (allocated) whose content's size is **different** than the one of the note's struct,  
  and then delete (free) them, the content's chunks will be inserted into `fastbin X`, while the note chunks will be inserted into `fastbin Y`.  
  Now, those fastbins look roughly like so:
  ```
  Fastbins
  -------------------------------------------------
  X: HEAD -> CONTENT CHUNK | CONTENT CHUNK -> TAIL
  -------------------------------------------------
  -------------------------------------------------
  Y: HEAD -> NOTE CHUNK | NOTE CHUNK -> TAIL
  -------------------------------------------------
  ```
  When another note will be created whose content's size is the **same** as the one of the note's struct,
  it would match the `Y` fastbin, a note chunk would be popped and returned for the content's allocation,  
  allowing us to write values to the note struct, rather than to the content's buffer.
- It is possible to overflow the heap completely using the note's content buffer which points to the note itself.



## Pseudo
```c
.bss:
	Note* notes[5]; // 0x0804A050
	int note_count; // 0x0804A04C


typedef struct {
	void* printer_func;
	char* content;
} Note
```


## Allocation Chain
```c
- malloc(sizeof(Note)); // fastbin (16)
- malloc(atoi(buf)); // controllable bin
```


## Defending
