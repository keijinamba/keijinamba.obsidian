
```dataview
TABLE
  file.link as "Note",
  file.folder as "Folder",
  image as "Preview"
FROM "WebClippings"
WHERE !contains(file.name, "Template")
SORT file.mtime desc
```
