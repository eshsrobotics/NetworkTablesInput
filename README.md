# NetworkTablesInput

This is a small utility library that the ESHS P.O.T.A.T.O.E.S. have been using
for the past few years to allow robots to be controlled with the keyboard.

It works by capturing keyboard and mouse input in a dedicated JavaFX window
and translating these into boolean values in NetworkTables, a sort of
distributed hashtable provided by WPILib.  Robot code can then read known key strings
from the `input` NetworkTable in order to figure out what the user has pressed.

Credit goes to @amclees for the original idea.

## Dependencies

This code has hard dependencies on NetworkTables and JavaFX.  Both must be
present in the `$CLASSPATH` in order to build and to run, and JavaFX must
_also_ be present in the `$MODULEPATH`.  The Gradle build normally takes care
of all this, producing a "fat JAR" with all of the dependencies included
within.

## How to build

To build this project and download all its dependencies, simply run:

``` shell
./gradlew build
```

from the current directory.  If all goes well and you have a solid Internet
connection, this should result in a build artifact called
`./build/distributions/networkworktablesinput.zip`.

## How to run

This program must be run from the driver station -- usually a Windows laptop.

1. To run NetworkTablesInput from the ZIP file, unzip it to any location, enter
  that folder, and run `bin/networktablesinput` (on Unix) or
  `bin/networktablesinput.bat` (on Windows) directly.

2. You can also execute:

    ``` shell
    ./gradlew run
    ```

  And the NetworkTablesInput program will be run in place.

3. Finally, if Gradle isn't working for whatever reason, you *can* run
  NetworkTablesInput by hand.  It's not easy, though:

    1. Download a Gluon SDK build of OpenJFX from https://gluonhq.com/products/javafx.
    2. Unzip the build into some folder, like `%USERPROFILE%\Downloads`.
    3. `cd` into the directory where `networktablesinput.jar` is located and run
    the following:

        ``` shell
        java -classpath "%USERPROFILE%/Downloads/javafx-sdk-11.0.2/lib/*;networktablesinput.jar" \
             --module-path="%USERPROFILE%/Downloads/javafx-sdk-11.0.2/lib" \
             --add-modules=javafx.controls,javafx.fxml \
             main.NetworkTablesInput
        ```

        changing the version number for the JavaFX SDK as appropriate.

        You're really better off sticking with Gradle.

### Troubleshooting

- _When I try to run, I get a `java.lang.UnsatisifedLinkError` in `ntcorejni.dll` that says "Can't find dependent libraries."_

  This is a [common problem](https://www.chiefdelphi.com/t/cant-start-shuffleboard/370646/2), 
  and it indicates that you need to install the latest
  [Visual C++ redistributable](https://aka.ms/vs/16/release/vc_redist.x64.exe).  This seems to only be a problem
  for people who use the offline WPILib installer. 

## How to use

- As NetworkTablesInput runs, keypresses and mouse clicks are captured in a
  blank JavaFX window and passed into the
  [edu.wpi.first.networktables.NetworkTable](https://first.wpi.edu/FRC/roborio/beta/docs/java/edu/wpi/first/networktables/NetworkTable.html)
  table called `inputTable` **as long as the window has the focus.** Every entry
  placed into the table is a boolean value indicating whether the key is
  pressed.  The keys themselves are just members of the
  [javafx.scene.input.KeyCode](https://openjfx.io/javadoc/11/javafx.graphics/javafx/scene/input/KeyCode.html)
  enumeration, converted to strings.  Examples include:

    | Keyboard key  | NetworkTables key string |
    | ------------- | ------------------------ |
    | Alphanumeric  | Uppercase of the key (i.e. `A`, `B`, `C`, ... or `0`, `1`, `2`, ...) |
    | Function keys | `F1`, `F2`, ... |
    | Shift         | `Shift` |
    | Control       | `Ctrl` |
    | Alt           | `Alt` |
    | Windows       | `Windows` |
    | Enter         | `Enter` |
    | Escape        | `Esc` |
    | `-`           | `Minus` |
    | `=`           | `Equals` |
    | `[`           | `Open Bracket` |
    | `]`           | `Close Bracket` |
    | `/`           | `Slash` |
    | `'`           | `Quote` |
    | `;`           | `Semicolon` |
    | Caps Lock     |  `Caps Lock` |
    | `\``         | `Back Quote`|

  Note that capturing the space bar is not possible because the drive station
  interprets this as an **emergency stop**.

- To use this code, you must initialize NetworkTables:

    ``` java
    import edu.wpi.first.networktables.NetworkTable;
    import edu.wpi.first.networktables.NetworkTableEntry;
    import edu.wpi.first.networktables.NetworkTableInstance;

    // Initialization

    NetworkTableInstance networkTableInstance = NetworkTableInstance.getDefault();
    if (networkTableInstance.isConnected()) {
      NetworkTable inputTable = networkTableInstance.getTable("inputTable");
      if (this.inputTable != null) {
        NetworkTableEntry upKey = inputTable.getEntry("Up")
        NetworkTableEntry downKey = inputTable.getEntry("Down")
        NetworkTableEntry leftKey = inputTable.getEntry("Left")
        NetworkTableEntry rightKey = inputTable.getEntry("Right")
        NetworkTableEntry zKey = inputTable.getEntry("Z")
        NetworkTableEntry xKey = inputTable.getEntry("x")
      }
    }

    ```

- Then during your polling loop, you can do something like this:

    ``` java
    if (upKey.getBoolean(false)) {
      // Drive robot forward
    } else if (downKey.getBoolean(false)) {
      // Drive robot backward
    } else {
      // Stop robot
    }

    if (leftKey.getBoolean(false)) {
      // Turn robot counterclockwise
    } else if (rightKey.getBoolean(false)) {
      // Turn robot clockwise
    }

    if (zKey.getBoolean(false)) {
      // Raise arm
    } else if (xKey.getBoolean(false)) {
      // Lower arm
    } else {
      // Stop arm motors
    }
    ```

## TODO

- [x] Add a Gradle build script so that we can pull the latest NetworkTables
  JAR the same way FRC robots do: `./gradlew build`.
- [x] Bring the batch file back to make this JAR easier to run.
  * The distribution created by `./gradlew build` includes a batch file.
- [ ] Add a section to this README describing how to read the
  NetworkTablesInput values from the robot code (as soon as we verify that
  _this_ code works.)
- [ ] Don't hard-code the team number; make it -- or, rather, the IP address
  that uses the team number -- configurable.
