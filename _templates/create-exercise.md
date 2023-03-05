<%*
/*
TODO - Query all values in exercise-records and generate graph of progress
TODO - Query all values in exercise-records and find personal best
TODO - look for a way to have global variables to configure instead of hard coding muscles
*/

const now = tp.date.now("YYYY-MM-DD:HH:mm:ss")

const exerciseName = await tp.system.prompt("Exercise Name")

const muscleGroups = [
"Abs",
"Biceps",
"Calves",
"Chest",
"Forearms",
"Glutes",
"Hamstrings",
"Lower Back",
"Middle Back / Lats",
"Neck",
"Obliques",
"Quadriceps",
"Shoulders",
"Triceps",
"Upper Back",
]
const primaryMuscle = await tp.system.suggester(muscleGroups, muscleGroups)

const loweredName = exerciseName.replace(/\W+/g, '-', '-').toLowerCase()

await tp.file.move("_database/exercises/" + loweredName);
-%>
---
name: <% exerciseName %>
type:
primary-muscle: <% primaryMuscle %>
secondary-muscles: []
tags: [exercise]
title: <% exerciseName %>
date-created: <% now %>
date-modified: <% now %>
---


# <% exerciseName %>


## Notes

## Images

## Workouts

```dataview
LIST 
FROM [[]] and #workout
```

## Exercise History

```dataview
LIST
FROM [[]] and #exercise-record
```
