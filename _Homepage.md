```button
name Create Workout
type note(temp-workout) template
action create-workout
templater true
```

```button
name Create Exercise
type note(temp-exercise) template
action create-exercise
templater true
```



Table of Workouts

```dataview
TABLE exercises
FROM "_database/workouts" and #workout and -#workout-record
```

Table of exercises

```dataview
TABLE primary-muscle, secondary-muscles
FROM "_database/exercises" and #exercise and -#exercise-record
```



Workout History
```dataview
TABLE
FROM "_database/workout-records"
```


```button
name Log Workout
type note(temp-log-workout) template
action create-workout-record
templater true
```
^button-k1z4
