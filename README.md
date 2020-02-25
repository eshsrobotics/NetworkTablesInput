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
insode it.

## How to build

To build this project and download all its dependencies, simply run:

``` shell
./gradlew build
```

from the current directory.  If all goes well and you have a solid Internet
connection, this should result in a build artifact called
`./build/distributions/networkworktablesinput.zip`.

## How to run

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

## TODO

- [x] Add a Gradle build script so that we can pull the latest NetworkTables
  JAR the same way FRC robots do: `./gradlew build`.
- [x] Bring the batch file back to make this JAR easier to run.
  * The distribution created by `./gradlew build` includes a batch file.
