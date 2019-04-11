FalconLibrary Units and Math
==============================

Summery
---------

FalconLibrary includes all common SI units and derived units as typesafe
objects. This includes base units such as Length, Time and Voltage, as
well as derived units such as Velocity and Acceleration.

All typesafe units include common mathematical operators such as urinary
plus, urinary minus, equivalency checks, multiplication and division.
These functions do
not modify the class on which they are called, but instead return a new
instance of it. So if I construct a new Length of 5 inches and call
:code:`.plus(x)` on it, that original length is still 5 inches.

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

Velocity
----------

Velocity, a derived unit of Length, is used to represent a linear or
angular speed.

.. tabs::
   .. code-tab:: java

      Velocity<Length> tenFeetPerSec = LengthKt.getFeet(10).div(TimeUnitsKt.getSecond(1));

   .. code-tab:: kotlin

      val tenFeetPerSec = 10.feet / 1.second

Acceleration
-------------

Acceleration, a derived unit of Velocity, is used to represent either
a linear or angular acceleration.

.. tabs::
   .. code-tab:: java

      Velocity<Length> tenFeetPerSecSquared = LengthKt.getFeet(10).div(TimeUnitsKt.getSecond(1)).div(TimeUnitsKt.getSecond(1));

   .. code-tab:: kotlin

      val tenFeetPerSecSquared = 10.feet / 1.second / 1.second


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
:code:`twist()`, teams are encourage to `Read the github source <https://github.com/5190GreenHopeRobotics/FalconLibrary/blob/32a9657467ad7866b9cca710cd937748f3c3aefb/src/main/kotlin/org/ghrobotics/lib/mathematics/twodim/geometry/Pose2d.kt>`_.

Twist2d
--------

Coming soon, i'm confused.

Twist2d holds a dx, dy and dtheta component to represent a robot "twist."
More docs coming soon.

Pose2dWithCurvature
---------------------

Pose2dWithCurvature, similar to Twist2d, holds :code:`Pose2d`
and curvature components. Curvature is devined as one over
the radius of a circle, and curvature can be positive or
negative depending on the direction that the pose twists -
left or right.

Other Units
------------

Other cool units which you might use include Ohms, Volts and Amps.




