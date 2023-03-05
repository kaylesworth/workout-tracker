<%*
const now = tp.date.now("YYYY-MM-DD:HH:mm:ss")

const fileName = await tp.system.prompt("Workout Name")

const loweredFileName = fileName.replace(/\W+/g, '-', '-').toLowerCase()

await tp.file.move("_database/workouts/" + loweredFileName);
-%>
---
name: <% fileName %>
date-created: <% now %>
date-modified: <% now %>
tags: [workout]
---

# <% fileName %>

exercises:: 

Workout History
```dataview
TABLE
from [[]] and #workout-record
```


