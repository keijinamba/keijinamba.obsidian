
```datacards
TABLE
	file.link as Note,
	file.folder as Folder,
	created,
	image
FROM "WebClippings"
WHERE !contains(file.name, "Template")
SORT file.mtime desc

// Settings
imageProperty: image
```
