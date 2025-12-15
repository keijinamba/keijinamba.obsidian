
```datacards
TABLE file.link as Note, file.folder as Folder, image
FROM "WebClippings"
WHERE !contains(file.name, "Template")
SORT file.mtime desc

// Settings
preset: portrait
imageProperty: image
```
