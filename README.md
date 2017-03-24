## Useful command lines in Linux (Ubuntu)

### Delete all empty files

```bash
find . -type f -size 0 -exec rm -f '{}' +
```

### Get first column in formatted data file

```bash
awk -F " " '{print $1}' data.txt
```

### Get lines from line 10 to line 20

```bash
head -n 20 data.txt | tail -n 10
```
