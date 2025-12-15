
```datacards
TABLE
	file.link as Note,
	file.folder as Folder,
	created,
	image
FROM #WebClipping 
WHERE !contains(file.name, "Template")
SORT file.mtime desc

// Settings
imageProperty: image
```
