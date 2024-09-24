# Class diagram
```mermaid
classDiagram
class User{
    -string userID
    -string userName
    -string gender
    -Date birthDay
    +string[] FriendIDs
    +Dictionary[string, string] appPrefrences
    +int MinutesPerDayGoal
    +int LevelScore
    +string GetUserId()
    +string GetUserName()
    +string GetGender()
    +DateOnly GetBirthDay()
}

User --> LongTermPlan : LongTermPlan CurrentLongTermPlan
class LongTermPlan{
    +string WorkoutName
    +int Progress
}

LongTermPlan --> IWorkoutPlan : IWorkoutPlan[] Workouts
<<interface>> IWorkoutPlan
class I WorkoutPlan{
    +string WorkoutName
    +int GetIntensityScore()
}

RunningWorkoutPlan ..|> IWorkoutPlan

class RunningWorkoutPlan{
    +float DurationKilometers
}
ThreadmillWorkoutPlan ..|> IWorkoutPlan 

class ThreadmillWorkoutPlan{
    +SpeedAndDuration[] Speeds
}


WalkingWorkoutPlan ..|> IWorkoutPlan 

class WalkingWorkoutPlan{
    +float DurationKilometers
}
FitnessWorkoutPlan ..|> IWorkoutPlan

class FitnessWorkoutPlan{
    +string Notes
}

class Workout{
    +int Time
    +int GetIntensityScore()
    +void UpdateAsCurrentWorkout(IHeartRateMonitor)
    +Dictionary[DateTime, float] HeartRateReadings
}
<<abstract>> Workout

class HeartRateMonitor{
    +void Activate()
    +void Deactivate()
    +void StartSession(int age)
    +int ReadCurrentHeartRate()
    +int GetAverageHeartRate()
}

class ProHeartBeater{
    +void StartTracking()
    +float GetHeartBeat()
    +float[] GetHeartBeats()
}

class IHeartRateMonitor{
    +void Initialize(int age)
    +float GetHeartRate()
}
<<interface>> IHeartRateMonitor

class HeartRateMonitorAdapter{
    
}
IHeartRateMonitor <|.. HeartRateMonitorAdapter
HeartRateMonitorAdapter *-- HeartRateMonitor

class ProHeartBeaterAdapter{
    
}
IHeartRateMonitor <|.. ProHeartBeaterAdapter
ProHeartBeaterAdapter *-- ProHeartBeater

class IMaps4All{
    +LoadMap()
    +StartTrack()
    +StopTrack()
    +PauseTrack()
    +GetPosition()
    +[..]()
}

Workout ..> IHeartRateMonitor

class Route{
    +Vector2[] RoutePoints
    +void TrackCurrentLocation()
}
Route --> IMaps4All

class RunningWorkout{
    +float DurationKilometers
    +float[] Speeds
}
Workout <|-- RunningWorkout 
RunningWorkout --> Route

class WalkingWorkout{
    +float DurationKilometers
    +float[] Speeds
}
Workout <|-- WalkingWorkout 
WalkingWorkout --> Route

class ThreadmillWorkout{
    +float[] Speeds
}

class ThreadmillEquipment
<<interface>> ThreadmillEquipment

Workout <|-- ThreadmillWorkout
ThreadmillWorkout --> ThreadmillEquipment

class FitnessWorkout{
    +int CustomSetIntensityScore
}

Workout <|-- FitnessWorkout
```
## LevelScore
This is a variable that tracks the userś level, to assist in
generating workouts. It remains invisible as to not rank people 
based on their fitness, going agains the friendly nature of the app. 

## GetIntensityScore()
Get a score judging the effort of a particular workout

## SpeedAndDuration
This type is used in the ThreadmillWorkoutPlan. 
We first wanted to use a tuple, but the UML software would'nt let us
type < or >. 
