LabVIEW Lyon Student Contest - Stopwatch
===========================

Competitor ID   : YOVO Yawo Emmanuel, florentyovo0@gmail.com
LabVIEW version : 2026 Q3, 64-bit


How to run
----------
1. Unzip the folder and keep all the files together
2. Open Stopwatch_Context_YOVO.lvproj
3. Open Main.vi and click Run
4. Use Start / Stop / Reset / Lap. Quit stops the program


What it does
------------
- Start begins the timing, Stop pauses it, and Start again resumes from where it stopped
- Reset puts everything back to 0. It only works when the stopwatch is paused
- The elapsed time is shown in hours, minutes, seconds and milliseconds
- Lap records the current elapsed time in the Laps table (only while running)
- The State indicator shows where we are: IDLE, RUNNING, PAUSED or FINISHED


How it works
------------
I use an event-driven state machine:

- Main.vi : one While loop with an Event Structure. Each button sends a
  command (START, STOP, RESET, LAP) to the Controller. When no button is hit,
  the 30 ms timeout only refreshes the display.
- Controller.vi : the state machine. It takes the data, the command and the
  current time, and gives back the new data. All the transitions are in this VI
- ControllerElapsedTime.vi : the only place where the elapsed time is computed
- Typedefs : the data cluster and the enums for the state, the mode and the command.

About the timing: we store the time of the last Start and the time accumulated before the last pause, so:

    elapsed = accumulated + (now - start time)

now term comes from High Resolution Relative Seconds. This way the accuracy doesn't
depend on the loop speed as it is not running on a RT device as myRIO or cRIO. The 30 ms timeout is only there for the display refresh.


Notes
-----
- The countdown mode is already handled in Controller.vi but not yet available
  on the front panel