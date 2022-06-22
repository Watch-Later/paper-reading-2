---
Alias: "compiler"
---

# compiler Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #compiler
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Papers

```dataview
table status
From #compiler and #paper
SORT status DESC
```

## Notes
