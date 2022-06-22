---
Alias: "detection"
---
# detection Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #detection
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Datasets

```dataview
table title
From #detection and #dataset
```

## Papers

```dataview
table status
From #detection and #paper
SORT status DESC
```

## Notes
