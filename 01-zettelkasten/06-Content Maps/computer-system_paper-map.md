---
Alias: "computer-system"
---

# computer-system Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #computer-system
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Papers

```dataview
table status
From #computer-system and #paper
SORT status DESC
```

## Notes
