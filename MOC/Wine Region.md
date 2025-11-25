---
up: [[MOC/Wine]]
related: 
created: "{{date}}"
in:
---

Below are the most recently created notes in the **Wine** folder.

```dataview
TABLE
  file.folder as "Parent Folder",
  length(file.outlinks) as "Links"
FROM #Wine
SORT file.mtime desc
LIMIT 100
```
