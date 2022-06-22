---
Alias: "programming"
---

# programming Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #programming
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Papers

```dataview
table status
From #programming and #paper
SORT status DESC
```

## Notes
