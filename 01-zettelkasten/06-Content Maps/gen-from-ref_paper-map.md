---
Alias: "gen-from-ref"
---
# gen-from-ref Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #gen-from-ref
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Datasets

```dataview
table title
From #gen-from-ref and #dataset
```

## Papers

```dataview
table status
From #gen-from-ref and #paper
SORT status DESC
```

## Notes
