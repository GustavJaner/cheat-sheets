_Author: Gustav Janér_

# Vim commands

copy and paste continuously from register:
yy - yank line
p  - paste

Several commands all store to the " (unnamed) register. 
So when deleting text using x or similar, this buffer(characters) will be stored in the " (unnamed) register and overwrite the previous yanked line.

Yanked text is also put in the 0 register automatically. 
regiter 0 is not afftected by delete or other change operations: use it to paste after intermediate deletes
"0p 
