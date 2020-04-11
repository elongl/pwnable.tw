## Information
- The notes are stored on the `.bss` section as `void* notes[5]` (max note count - 5).
- Each note points to a note struct that consists of a printer function, and the content itself which is allocated on the heap.


## Thoughts & Leads
- Setting the printer function to `system`, and the note to `"/bin/sh"` and print the node.