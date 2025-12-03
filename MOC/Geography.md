---
up:
related: 
created: "{{date}}"
in:
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
