# Other-default Paper Map

## Overview

```dataview
table length(rows.file.link) AS Count
From #other-default
FLATTEN file.tags AS tag
GROUP BY tag
SORT length(rows.file.link) DESC
```

## Papers

```dataview
table status
From #other-default and #paper
```
