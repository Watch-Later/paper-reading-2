---
Alias: "archive"
---

# archive Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #archive
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Papers

```dataview
table status
From #archive and #paper
SORT status DESC
```

## Notes
