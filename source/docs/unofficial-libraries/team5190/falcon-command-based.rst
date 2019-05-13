FalconLibrary's Command-based Implememntation
======================================================

FalconLibrary implements a Command Based framework, similar
to WPILib. This command based implementation is based upon
wrapping WPILib's Command based Commands and Subsystems, but
is scheduled based upon Kotlin Coroutines and includes
syntactic sugar for command group building. This example
from Team 5190's 2018 offseason code demonstrates building
CommandGroups with both parallel and sequential commands.

.. code-block:: kotlin

    // Place third cube in scale
    +parallel { // run all these commands in parallel
        +DriveSubsystem.followTrajectory(cube2ToScale, shouldMirrorPath)
            .withExit(stopScalePathCondition)
        +sequential { // run first the DelayCommand, then move the arm back
            +DelayCommand(cube2ToScale.lastState.t - 2.7.second)
            +SubsystemPreset.BEHIND.command
        }
        +sequential { // wait for the arm, then outtake a cube.
            +ConditionCommand { ArmSubsystem.armPosition > Constants.kArmBehindPosition - Constants.kArmAutoTolerance }
            +IntakeCommand(IntakeSubsystem.Direction.OUT, 0.4).withTimeout(500.millisecond)
        }
    }



.. note:: To use FalconLibrary's built-in tank-drive drivetrains a team will have to be completely Falcon-command-based, and teams using WPI's command-based cannot use any FalconCommands or FalconSubsystems.
