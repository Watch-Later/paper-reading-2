---
Alias: "noisy-label"
---
# noisy-label Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #noisy-label
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Datasets

```dataview
table title
From #noisy-label and #dataset
```

## Papers

```dataview
table status
From #noisy-label and #paper
SORT status DESC
```

## Notes
