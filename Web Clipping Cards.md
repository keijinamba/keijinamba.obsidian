
```datacards
TABLE
	site,
	created,
	favicon,
	image
FROM #WebClipping 
WHERE !contains(file.name, "Template")
SORT created desc

// Settings
imageProperty: image
```
