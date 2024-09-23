# Class diagram
```mermaid
classDiagram
class User{
    -string userID
    -string userName
    -string gender
    +string[] FriendIDs
    +Dictionary<string, string> appPrefrences
    +int MinutesPerDayGoal
    +int LevelScore
    +string GetUserId()
    +string GetUserName()
    +string GetGender()
}

User --> LongTermPlan : LongTermPlan CurrentLongTermPlan
class LongTermPlan{
    +string WorkoutName
    +int Progress
}

LongTermPlan --> WorkoutPlan : WorkoutPlan[] Workouts
<<interface>> WorkoutPlan
class WorkoutPlan{
    +string WorkoutName
    +int GetIntensityScore()
}

RunningWorkoutPlan ..|> WorkoutPlan

class RunningWorkoutPlan{
    +float DurationKilometers
}
ThreadmillWorkoutPlan ..|> WorkoutPlan 

class ThreadmillWorkoutPlan{
    +SpeedAndDuration[] Speeds
}


WalkingWorkoutPlan ..|> WorkoutPlan 

class WalkingWorkoutPlan{
    +float DurationKilometers
}
FitnessWorkoutPlan ..|> WorkoutPlan

class FitnessWorkoutPlan{
    +string Notes
}
```
## LevelScore
This is a variable that tracks the userś level, to assist in
generating workouts. It remains invisible as to not rank people 
based on their fitness, going agains the friendly nature of the app. 

## GetIntensityScore()
Get a score judging t

## SpeedAndDuration
This type is used in the ThreadmillWorkoutPlan. 
We first wanted to use a tuple, but the UML software would'nt let us
type < or >. 