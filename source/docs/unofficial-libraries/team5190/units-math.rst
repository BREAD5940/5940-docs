FalconLibrary Units and Math
==============================

Summary
---------

FalconLibrary includes all common SI units and derived units as typesafe
objects. This includes base units such as Length, Time and Voltage, as
well as derived units such as Velocity and Acceleration.

All typesafe units include common mathematical operators such as unary
plus, unary minus, equivalency checks, multiplication and division.

.. note::
   To use these functions in Java, one must call :code:`Rotation2dKt.getDegree(10).div(TimeUnitsKt.getSecond(10))`
   rather than :code:`Rotation2dKt.getDegree(10) / TimeUnitsKt.getSecond(10)`.
   This does not apply to Kotlin users.


.. note::
   These functions do
   not modify the class on which they are called, but instead return a new
   instance of it. So if I construct a new Length of 5 inches and call
   :code:`.plus(x)` on it, that original length is still 5 inches.

All the base units contain the basic SI increments, such as milli-,
micro-, nano, as well as kilo- or even yotta- and exa-.
Additionally, units include (in general) the imperial equivalent,
such as inches and feet or miles for Length or pounds for Mass.
See your autocomplete for a full list of types.

Time
-----

Time represents a time, and implements SIUnit. Use this to represent a
passing time, a duration, or to construct derived units such as `Velocity`_
and `Acceleration`_.'

.. tabs::
   .. code-tab:: java

      Time aTime = TimeUnitsKt.getSecond(10);

   .. code-tab:: kotlin

      val aTime = 10.second

      val anotherTime = 10.millisecond

Length
--------

A Length represents a displacement in 1D space, and can be either positive
or negative. The function :code:`aLength.getValue()` will return the value
in meters, as it is the base unit of Length in the SI system.

.. tabs::
   .. code-tab:: java

        // creating a Length
        Length aLength = LengthKt.getFeet(10);
        Length anotherLen = LengthKt.getMeter(3);

        // getting a Length
        double aLengthInInches = aLength.getInch();
        double miles = anotherLen.getMile();

   .. code-tab:: kotlin

        val aLen = 10.feet
        val anotherLen = 3.meter

        val inches = aLen.inch
        val miles = anotherLen.mile

Rotation2d
-------------

A Rotation2d represents a rotation in 2d space. Think of it like an angle
on a unit circle - it can represent the angle of the triangle's hypotenuse,
and can be converted into a `Translation2d`_ with X and Y components
correlated to the angle's sine and cosine components.

.. tabs::
   .. code-tab:: java

      Rotation2d rotation = Rotation2dkt.getDegree(45);

      // returns true
      var isParallel = rotation.isParallel(rotation);

      Rotation2d aMultiple = rotation.times(4);

      Rotation2d mMinus = rotation.minus(Rotation2dKt.getDegree(30);

      Rotation2d mPlus = rotation.plus(Rotation2dKt.getDegree(-10);

   .. code-tab:: kotlin

      val rotation = 45.degree

      val isParallel = rotation.isParallel(rotation)

      val aMultiple = rotation.times(4)

      val mMinus = rotation.minus(30.degree)

      val mPlus = rotation.plus((-10).radian)

NativeUnit
------------

NativeUnit, an SIValue, are often used on motor controllers with feedback
sensors,
such as TalonSRXes with Quadrature encoders. These encoders output a
fixed number of pulses for every rotation of the shaft they are connected
to, and a NativeUnit represents these pulses distinct from information
encoding actual, real-life position measurements such as distance or
angles. Because NativeUnit is a SIValue, it inherits the same common
operators as Length and Rotation2d.
Conversion between NativeUnits and physical units are done
using the Native Unit Model abstract class. Included in FalconLibrary
are :code:`NativeUnitLengthModel` and :code:`NativeUnitRotationModel`
models, which covers both linear applications (for example, an elevator
or slide) and angular applications (such as an arm). These models
include methods to convert between native and physical unit positions,
velocities, accelerations, and error, among other things.

.. warning::
      The TalonSRX encodes velocity as ticks per 100ms, however
      other motor controllers such as the Spark Max encode rpm
      by default. Furthermore, most motor controllers will let
      you multiply their measured ticks by an arbitrary constant,
      so even if the Spark MAX says RPM, you may have it configured
      for RPS. Keep this in mind when using Length and Rotation
      models!


.. tabs::
   .. code-tab:: java

      NativeUnit someUnits = NativeUnitKt.getNativeUnits(10);

   .. code-tab:: kotlin

      val nativeUnits = 10.nativeUnits


Velocity
----------

Velocity, a derived unit, is often used to represent a linear or
angular speed. However it is possible to make a Velocity of any
type that impalements SIValue. The type of Velocity represented can be
parameterized by any class that implements SIValue - for instance, a
:code:`Velocity<Length>`, or :code:`Velocity<Rotation2d>`, or even
:code:`Velocity<NativeUnit>`.

.. tabs::
   .. code-tab:: java

      // a linear velocity
      Velocity<Length> tenFeetPerSec = LengthKt.getFeet(10).div(TimeUnitsKt.getSecond(1));

      // an angular velocity
      Velocity<Rotation2d> tenDegPerSec = Rotation2dKt.getDegree(10).div(TimeUnitsKt.getSecond(1));

      double radPerSec = tenDegPerSec.getType$FalconLibrary().getRadian();

      Velocity<NativeUnit> ticksPerSec = NativeUnitKt.getNativeUnits(10).div(TimeUnitsKt.getSecond(1));

   .. code-tab:: kotlin

      val tenFeetPerSec = 10.feet / 1.second

      val tenDegPerSec = 10.degree / 1.second

      val ticksPerSec = 10.nativeUnits / 1.second

      // TODO make this actually work in kotlin
      val inRadiansPerSec = aVel.getType$FalconLibrary().getRadian();

Acceleration
-------------

Acceleration, a derived unit of Velocity, is used to represent either
a linear or angular acceleration. Similar to Length, the type can be
parameterized by any class that implements SIValue. Similar to Length,
Acceleration must be parameterized by a class with inherits SIValue.

.. tabs::
   .. code-tab:: java

      Velocity<Length> tenFeetPerSecSquared = LengthKt.getFeet(10).div(TimeUnitsKt.getSecond(1)).div(TimeUnitsKt.getSecond(1));

      Velocity<Rotation2d> nyooooomAccel = Rotation2dKt.getDegree(10000).div(TimeUnitsKt.getSecond(1)).div(TimeUnitsKt.getSecond(1));

      Velocity<NativeUnit> fastNativeUnitNyoom = NativeUnitKt.getNativeUnits(10000).div(TimeUnitsKt.getSecond(1)).div(TimeUnitsKt.getSecond(1));

   .. code-tab:: kotlin

      val tenFeetPerSecSquared = 10.feet / 1.second / 1.second

      val angularAccel = 10000.degree / 1.second / 1.second

      val fastNativeUnitNyoom = 1000000.nativeUnits / 1.second / 1.second

Translation2d
---------------

A Translation2d is similar to a 2d vector. It can be constructed
either with a typesafe magnitude and direction, or from x
and y components, or from the displacement between two other
Translation2ds. Translation2d is also special because it implements
VaryInterpolatable, which means that you can linearly interpolate
between two Translation2ds. This is very useful for path following.

.. tabs::
   .. code-tab:: java

      // This is assumed to be meters
      Translation2d tran = new Translation2d(
            4, 5
      );

      // This is a typesafe translation
      tran = new Translation2d(
            LengthKt.getInch(4),
            LengthKt.getFeet(10)
      );

      // make a Translation2d out of essentially a vector
      tran = new Translation2d(
            LengthKt.getFeet(20),
            Rotation2dKt.getDegree(21)
      );

      // This will have a "norm" of 1 meter
      Translation2d anotherTran = Translation2dKt.fromRotation(Rotation2dkt.getDegree(45));

      // return the point interpolated half way between these two points
      var interpolated = tran.interpolate(anotherTran, 0.5);

      // get the Length of the hypotenuse of this
      var hypotenuseLength = tran.norm();

   .. code-tab:: kotlin

      // This is assumed to be meters
      val tran = Translation2d(4, 5);
      val tran = Translation2d(4.feet, 10.meter)

      // make a Translation2d out of essentially a vector
      val tran = Translation2d(5.feet, 21.degree)

      // This will have a "norm" of 1 meter
      val anotherTran = Translation2d.fromRotation(45.degree)

      // return the point interpolated half way between these two points
      val interpolated = tran.interpolate(anotherTran, 0.5);

      // get the Length of the hypotenuse of this
      val hypotenuseLength = tran.norm()

Pose2d
-------

Pose2d is a composition of Translation2d and Rotation2d. It represents
a point in 2 dimensional space with an associated heading, for example,

.. tabs::
   .. code-tab:: java

      var pose = new Pose2d(LengthKt.getInch(5), LengthKt.getInch(5), Rotation2dKt.getDegree(45);

   .. code-tab:: kotlin

      val pose = Pose2d(Translation2d(5.feet, 2.inch), 45.degree)

This unit is also really useful for path following, and is used to
represent a robot's 2d position on the field and a heading. The type
also includes methods such as :code:`.mirror()`, which mirrors the Pose2d
about the middle of the field (left/right, relative to the alliance wall),
and the usual plus/minus functions, and interpolation methods. For
more advanced functions such as :code:`inFrameOfReferenceOf()` or
:code:`twist()`, teams are encourage to `Read the github source <https://github.com/5190GreenHopeRobotics/FalconLibrary/blob/32a9657467ad7866b9cca710cd937748f3c3aefb/src/main/kotlin/org/ghrobotics/lib/mathematics/twodim/geometry/Pose2d.kt>`__.

Twist2d
--------

Coming soon, i'm confused.

Twist2d holds a dx, dy and dtheta component to represent a robot "twist."
More docs coming soon.

Pose2dWithCurvature
---------------------

Pose2dWithCurvature, similar to Twist2d, holds :code:`Pose2d`
and curvature components. Curvature is defined as one over
the radius of a circle, and curvature can be positive or
negative depending on the direction that the pose twists -
left or right.

Other Units
------------

Other SI Units of FalconLibrary not men sioned here include
Mass, Ohms, Volts and Amps.




