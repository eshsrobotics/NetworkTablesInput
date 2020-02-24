# NetworkTablesInput

This is a small utility library that the ESHS P.O.T.A.T.O.E.S. have been using
for the past few years to allow robots to be controlled with the keyboard.

It works by capturing keyboard and mouse input in a dedicated JavaFX window
and translating these into boolean values in NetworkTables, a sort of
distributed hashtable provided by WPILib.  Robot code can then read known key strings
from the `input` NetworkTable in order to figure out what the user has pressed.

Credit goes to @amclees for the original idea.

## Dependencies

This code has hard dependencies on NetworkTables and JAvaFX.  Both must be
present in the `$CLASSPATH` in order to build and to run.

## TODO list

- [ ] Add a Gradle build script so that we can pull the latest NetworkTables
  JAR the same way FRC robots do: `./gradlew build`.
- [ ] Bring the batch file back to make this JAR easier to run.
