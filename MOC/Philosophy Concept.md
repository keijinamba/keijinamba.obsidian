---
up: "[[MOC/Philosophy]]"
related: 
created: 2026/01/04
in:
  - "[[MOC/Philosophy]]"
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

