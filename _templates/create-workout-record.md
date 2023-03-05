<%*
const today = tp.date.now("YYYY-MM-DD-HH.mm")
const now = tp.date.now("dddd, MMMM Do YYYY, h:mm a")

const dv = this.app.plugins.plugins["dataview"].api

// Also filter by tag if a shared folder of all data instead of subfolders
const workouts = dv.pages('"_database/workouts" and #workout and -#workout-record')

const selectedWorkout = await tp.system.suggester(workouts.map(b => b.file.name), workouts)

// Failsafe for if workout has only one exercise
const exercises = Array.isArray(selectedWorkout.exercises) ? selectedWorkout.exercises : [selectedWorkout.exercises]


const workoutName = selectedWorkout.name.replace(/\W+/g, '-', '-').toLowerCase()

let title = `${selectedWorkout.name} - Workout - ${today}`

await tp.file.move(`_database/workout-records/${workoutName}-${today}`);
-%>
---
tags: [workout-record]
title: <% title %>
date-created: <% now %>
date-modified: <% now %>
---
# <% title %>

exercises:: <% selectedWorkout.exercises %>
workout:: <% selectedWorkout.file.link %>

```button
name Log Exercise
type note(temp-exercise-record) template
action create-exercise-record
templater true
```


## Todays Workout

```dataview
TABLE exercise, reps, sets, weight, time, distance
FROM [[]] and #exercise-record 
```

## Last Workout

<%*


const queryString = `
TABLE WITHOUT ID exercise, reps, sets, weight, time, distance
FROM "_database/exercise-records" and #exercise-record
SORT date-created desc
`

const pathNames = []
for (let i = 0; i < exercises.length; i++) {
	pathNames.push(exercises[i].path)
}

const query = await dv.query(queryString)

const headers = query.value.headers
const tableRows = query.value.values

const relevantRows = tableRows.filter(e => pathNames.includes(e[0].path))

const set = new Set()
const filteredRows = []
relevantRows.map((e) => {
	// This has a hard assumption again the queryString, if that order changes this will need to as well to follow the link to the exercise
	const path = e[0].path
	const isDup = set.has(path)
	set.add(path)
	if (!isDup) {
		filteredRows.push(e)
	}
})

const markdownTable = dv.markdownTable(headers, filteredRows)

tR += markdownTable

-%>
