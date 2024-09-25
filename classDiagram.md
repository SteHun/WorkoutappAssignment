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
    -Date birthDay
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

class SavedThreadmillWorkout{
    +float DurationKilometers
    +float[] Speeds
}

SavedWorkout <|-- SavedThreadmillWorkout

class SavedRunningWorkout{
    +float DurationKilometers
    +float[] Speeds
}

class SavedFitnessWorkout{
    +string Notes
}

SavedWorkout <|-- SavedFitnessWorkout

SavedWorkout <|-- SavedRunningWorkout

SavedRunningWorkout o--> Route : +Route RouteTaken

class SavedWalkingWorkout{
    +float DurationKilometers
    +float[] Speeds
}

SavedWorkout <|-- SavedWalkingWorkout

SavedWalkingWorkout o--> Route : +Route RouteTaken


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
    +int TimeDuration
    +DateTime TimeStarted
    +int GetIntensityScore()
    +void UpdateAsCurrentWorkout(IHeartRateMonitor)
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

class IHeartRateMonitor{
    +void Initialize(int age)
    +float GetHeartRate()
}
<<interface>> IHeartRateMonitor

class HeartRateMonitorAdapter{
    
}
IHeartRateMonitor <|.. HeartRateMonitorAdapter
HeartRateMonitorAdapter *-- HeartRateMonitor : -HeartRactMonitor monitor

class ProHeartBeaterAdapter{
    
}
IHeartRateMonitor <|.. ProHeartBeaterAdapter
ProHeartBeaterAdapter *-- ProHeartBeater : -ProHeartBeater monitor

class IMaps4All{
    +LoadMap()
    +StartTrack()
    +StopTrack()
    +PauseTrack()
    +GetPosition()
    +[..]()
}

Workout ..> IHeartRateMonitor : Call

class Route{
    +Vector2[] RoutePoints
    +void TrackCurrentLocation()
}
Route *-- IMaps4All : -IMaps4All mapService

class RunningWorkout{
    +float DurationKilometers
    +float[] Speeds
}
Workout <|-- RunningWorkout 
RunningWorkout o--> Route : +Route RouteTaken

class WalkingWorkout{
    +float DurationKilometers
    +float[] Speeds
}
Workout <|-- WalkingWorkout 
WalkingWorkout o--> Route : +Route RouteTaken

class FitnessWorkout{
    +int CustomSetIntensityScore
    +string Notes
}
Workout <|-- FitnessWorkout

class ThreadmillWorkout{
    +float DurationKilometers
    +float[] Speeds
}

class Equipment
<<interface>> Equipment



class ThreadmillEquipment
<<interface>> ThreadmillEquipment

Workout <|-- ThreadmillWorkout
ThreadmillWorkout o--> ThreadmillEquipment : +ThreadmillEquipment Threadmill
User o--> Equipment : +Equipment[] RegisterredEquipment
Equipment ..|> ThreadmillEquipment

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