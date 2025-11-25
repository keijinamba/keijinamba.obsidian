---
up: "[[MOC/Wine]]"
related: 
created: "{{date}}"
in:
  - "[[MOC/Wine]]"
---

```dataview
TABLE
  file.folder as "Parent Folder",
  length(file.outlinks) as "Links"
WHERE
	contains(in,this.file.link) and
	!contains(file.name, "Template")
SORT file.mtime desc
```
