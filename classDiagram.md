# Class diagram
```mermaid
classDiagram

class Controller
<<static>> Controller
Controller o--> User : -User currentUser
Controller o--> Workout : -Workout currentWorkout

class FriendlyMessages{
    +Dictionary[string, string[]] positiveMessages
    +Dictionary[string, string[]] encouragingMessages
    +string RandomPositiveMessage()
    +string RandomEncouragingMessage()
}
<<static>> FriendlyMessages

class User{
    -string userID
    -string userName
    -string gender
    -Date birthday
    +string[] FriendIDs
    +Dictionary[string, string] appPrefrences
    +int MinutesPerDayGoal
    +int LevelScore
    +string GetUserId()
    +string GetUserName()
    +string GetGender()
    +DateOnly GetBirthDay()
    +Workout[] GeneratePersonallizedWorkouts()
}


class SavedWorkout{
    +int TimeDuration
    +DateTime TimeStarted
    +int GetIntensityScore()
    +Dictionary[DateTime, float] HeartRateReadings
    +Image ExportAsImage()
}
<<abstract>> SavedWorkout

class SavedTreadmillWorkout{
    +float DurationKilometers
    +float[] Speeds
}

SavedWorkout <|-- SavedTreadmillWorkout

class SavedRunningWorkout{
    +float DurationKilometers
    +float[] Speeds
}

class SavedFitnessWorkout{
    +string Notes
}

SavedWorkout <|-- SavedFitnessWorkout

SavedWorkout <|-- SavedRunningWorkout

SavedRunningWorkout o--> Route: +RouteTracker RouteTaken

class SavedWalkingWorkout{
    +float DurationKilometers
    +float[] Speeds
}

SavedWorkout <|-- SavedWalkingWorkout

SavedWalkingWorkout o--> Route: +RouteTracker RouteTaken


User o--> Post : +Post[] Posts

class Post{
    +string Title
    +int Likes
}
Post o--> SavedWorkout

User o--> ActivityLog : +ActivityLog ActivityLog
class ActivityLog{
    +int[] GetWeeklyIntensityScores()
}

ActivityLog o--> SavedWorkout : +Workout[] CompletedWorkouts

User o--> LongTermPlan : +LongTermPlan CurrentLongTermPlan
class LongTermPlan{
    +string WorkoutName
    +int Progress
    +static LongTermPlan ImportFromFile(string filename)
}

LongTermPlan o--> IWorkoutPlan : +IWorkoutPlan[] Workouts
<<interface>> IWorkoutPlan
class IWorkoutPlan{
    +string WorkoutName
    +int GetIntensityScore()
}

RunningWorkoutPlan ..|> IWorkoutPlan

class RunningWorkoutPlan{
    +float DurationKilometers
}
TreadmillWorkoutPlan ..|> IWorkoutPlan 

class TreadmillWorkoutPlan{
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
    +int TimeDuration
    +DateTime TimeStarted
    +int GetIntensityScore()
    +void UpdateAsCurrentWorkout()
    +void PauseWorkout()
    +void ResumeWorkout()
    +void StopWorkout()
    +SavedWorkout GenerateSavedWorkout()
    +Dictionary[DateTime, float] HeartRateReadings
}
<<abstract>> Workout

Workout ..> SavedWorkout : Create

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

class RunningWorkout{
    +float DurationKilometers
    +float[] Speeds
}

class IHeartRateMonitor{
    +void Initialize(int age)
    +float GetHeartRate()
}
<<interface>> IHeartRateMonitor

class HeartRateMonitorAdapter{
    
}
HeartRateMonitorAdapter ..|> IHeartRateMonitor
HeartRateMonitorAdapter *-- HeartRateMonitor : -HeartRactMonitor monitor


class ProHeartBeaterAdapter{
    
}
IHeartRateMonitor <|.. ProHeartBeaterAdapter
ProHeartBeaterAdapter *-- ProHeartBeater : -ProHeartBeater monitor


Workout --> IHeartRateMonitor : -IHeartRateMonitor HeartRateMonitor

class Route{
    +Vector2[] RoutePoint
}

class RouteTracker{
    +Vector2[] RoutePoints
    +void TrackCurrentLocation()
}

RouteTracker o--> Route : +Route TrackedRoute


Workout <|-- RunningWorkout 
RunningWorkout o--> RouteTracker : +RouteTracker RouteTaken

class WalkingWorkout{
    +float DurationKilometers
    +float[] Speeds
}
Workout <|-- WalkingWorkout 
WalkingWorkout o--> RouteTracker : +RouteTracker RouteTaken

class FitnessWorkout{
    +int CustomSetIntensityScore
    +string Notes
}
Workout <|-- FitnessWorkout

class TreadmillWorkout{
    +float DurationKilometers
    +float[] Speeds
}

class IEquipment{
    +string EquipmentName
    +string ManufacturerName
}
<<interface>> IEquipment



class ITreadmillEquipment
<<interface>> ITreadmillEquipment

Workout <|-- TreadmillWorkout
TreadmillWorkout o--> ITreadmillEquipment : +ITreadmillEquipment Treadmill
User o--> IEquipment : +IEquipment[] RegisterredEquipment
IEquipment ..|> ITreadmillEquipment

RouteTracker *-- IMaps4All : -IMaps4All mapService
class IMaps4All{
    +LoadMap()
    +StartTrack()
    +StopTrack()
    +PauseTrack()
    +GetPosition()
    +[..]()
}

```
## LevelScore
This is a variable that tracks the user's level, to assist in
generating workouts. It remains invisible as to not rank people 
based on their fitness, going agains the friendly nature of the app. 

## GetIntensityScore()
Get a score judging the effort of a particular workout

## SpeedAndDuration
This type is used in the ThreadmillWorkoutPlan. 
We first wanted to use a tuple, but the UML software would'nt let us
type < or >. 

## FriendlyMessages
This is a static class wich could be used by the UI. 
We are including it because it was a simple addition
and showed up often in the document. 

## ThreadmillEquipment
We want the app to be ready to support threadmills, 
but we don't have access to usable API's at the moment. 
Therefore, we designated an interface for them that remains empty for now. 