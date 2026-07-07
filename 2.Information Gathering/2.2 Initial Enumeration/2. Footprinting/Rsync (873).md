
```bash
#Lists files and folders at a given path from an rsync server, without downloading them.
rsync -av --list-only rsync://127.0.0.1/dev
```

**Përdorimi:**

- Enumerim file/folders në një rsync server
- Zbulim të shares të ekspozuara
- Testim access read/write