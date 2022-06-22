---
Alias: "engineering"
---
# engineering Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #engineering
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Datasets

```dataview
table title
From #engineering and #dataset
```

## Papers

```dataview
table status
From #engineering and #paper
SORT status DESC
```

## Notes
