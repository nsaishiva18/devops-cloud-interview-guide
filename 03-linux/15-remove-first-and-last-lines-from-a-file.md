## Question  
How do you remove the first and last line of a file using `sed`?

### 📝 Short Explanation  
This task checks your understanding of line-addressing in `sed`, which is a powerful stream editor used in shell scripting for text manipulation.

## ✅ Answer  

### 🖥️ Command:

```bash
sed '1d; $d' filename.txt # This will print the oupput.
sed -i '1d $d' filename.txt # This will edit the file in place.
```

---

### 📘 Detailed Explanation

#### 🔍 Breakdown:
- `sed`: Stream editor used to process text line-by-line.
- `'1d'`: Deletes the **first line** (`1` = line number).
- `'$d'`: Deletes the **last line** (`$` = end of file).
- `filename.txt`: The file to be processed.

Together, the command says:
> "Delete the first line **and** the last line from `filename.txt`."

---

### ✅ Example:

If `file.txt` contains:
```
Line 1
Line 2
Line 3
Line 4
Line 5
```

Command:
```bash
sed '1d; $d' file.txt
```

Output:
```
Line 2
Line 3
Line 4
```

---

### 🧠 Bonus Tip:
If you want to **save the result to a new file**:
```bash
sed '1d; $d' file.txt > trimmed.txt
```

> Summary:  
> The `sed '1d; $d'` command is an efficient way to remove both the first and last lines of a text file using line number and end-of-file markers.

---
