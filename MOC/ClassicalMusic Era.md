---
up: "[[MOC/ClassicalMusic]]"
related: 
created: 2025/12/20
in:
  - "[[MOC/ClassicalMusic]]"
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
