# Zip
## Password Protected 
```bash
zip2john FILE.zip > hash
```
```bash
john hash
```
# Swap
Open the File with Vim:
```bash
vim -r FILE.EXT.swp
```
Read Strings 
```bash
strings FILE.EXT.swp > FILE
```
Read Strings (in Reverse)
```bash
strings FILE.EXT.swp > FILE; tac FILE
```
