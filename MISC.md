## get records types
```
fetch recordtypes
| summarize count(), by: {superType,name}
```