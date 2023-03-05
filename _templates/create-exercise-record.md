<%*
const today = tp.date.now("YYYY-MM-DD-HH.mm")
const now = tp.date.now("YYYY-MM-DD:HH:mm:ss")

const files = app.workspace.getLastOpenFiles();
const dv = this.app.plugins.plugins["dataview"].api

// This should in theory grab the Workout Record if following the expected format but would break if creating an exercise record is done elsewhere
const lastFile = dv.page(files[0])

// Double check exercises is an array from the last file (happens when only one exercise exists)
const exercises = Array.isArray(lastFile.exercises) ? lastFile.exercises : [lastFile.exercises]

const workout = lastFile.workout

const selectedExercise = await tp.system.suggester(exercises.map(e => e.path.split(/[\/]+/).pop()), exercises)

// Get the last path of file path by splitting / and then remove file extension and replace spaces with hyphens
const fileNameWithExtension = selectedExercise.path.split(/[\/]+/).pop()
const fileName = fileNameWithExtension.replace(/\.[^/.]+$/, "")
const loweredFileName = fileName.replace(/\W+/g, '-', '-').toLowerCase()

const title = `${fileName} - ${today}`

await tp.file.move(`_database/exercise-records/${loweredFileName}-${today}`);

-%>
---
tags: [exercise-record]
date-created: <% now %>
date-modified: <% now %>
date: <% tp.date.now("YYYY-MM-DD") %>
title: <% title %>
---

# <% title %>

Date::
Exercise:: <% selectedExercise %>
Workout:: <% lastFile.file.link %>
Type::
Sets::
Reps::
Weight::
Time::
Distance::
