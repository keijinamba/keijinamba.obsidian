---
up: "[[MOC/Literature]]"
related: 
created: 2026/01/04
in:
  - "[[MOC/Literature]]"
---

```dataview
TABLE
  file.folder as "Parent Folder",
  length(file.outlinks) as "Links"
WHERE
	contains(in, this.file.link) and
	!contains(file.name, "Template")
SORT file.mtime desc
```

