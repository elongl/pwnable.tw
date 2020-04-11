## Information
- The notes are stored on the `.bss` section as `void* notes[5]` (max note count - 5).
- Each note points to a note struct that consists of a printer function, and the content itself which is allocated on the heap.
- Deleting a note frees up the memory of the note, but doesn't decrement the note counter, nor nulls the pointers on the notes array.
- Value that is returned from `atoi` is signed, and then passed to `malloc` which is unsigned.
- Printing a note after it was deleted attempts to call the printer function on the content, both of which had already been freed, causing a segfault (use-after-free).
- The same note can be freed twice (segfault) because the pointer isn't being nulled (use-after-free).


## Thoughts & Leads
- Setting the printer function to `system`, and the note to `"/bin/sh"` and print the node.

## Goals
- Overwrite the printer function.