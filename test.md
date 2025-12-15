
```datacards
TABLE
	file.link as Note,
	file.folder as Folder,
	created,
	image
FROM #WebClipping 
WHERE !contains(file.name, "Template")
SORT created desc

// Settings
imageProperty: image
```
