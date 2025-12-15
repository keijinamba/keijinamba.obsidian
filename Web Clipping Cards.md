
```datacards
TABLE
	site,
	author,
	created,
	image
FROM #WebClipping 
WHERE !contains(file.name, "Template")
SORT created desc

// Settings
imageProperty: image
```
