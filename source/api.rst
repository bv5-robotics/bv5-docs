=============
API Reference
=============

Global Variables
----------------

.. c:var:: uint32_t systemVersion

    The current version of VEXos

.. c:var:: uint32_t stdlibVersion

    The current version of the stdlib that is being used

.. c:var:: uint32_t sdkVersion

    The current version of the sdk that is being used

.. c:var:: uint64_t startupTime

    The time that the system was powered on

Robot Management
----------------

.. c:function:: void delay(uint32_t ms)

    Delay the currently running task for a period of time

    :param ms: The number of milliseconds to delay (or 0 for as short
               as possible)

.. c:function:: bool isDriver()

    Check if we're in driver control or not

.. c:function:: bool isAuton()

    Check if we're in auton mode or not

.. c:function:: bool isCompetition()

    This will be true when we are in competiton mode (either field
    control or a testing controller are connected).

.. c:function:: bool fieldConnected()

    This will be true if we are connected to a field, but false if we
    are at inspection or using a testing controller.

    .. warning::
        This **clearly** has a lot of room for abuse. Do what you
        will with it, but it's not my fault if you do something
        stupid and get DQ'd.

.. c:function:: void exit(int code)

    Quit the program.

    :param code: The exit code. This isn't actually used, just added
                 for compatability with the normal ``exit`` function.

.. c:function:: uint32_t getMillis()

    Get the current time in milliseconds

.. c:function:: uint64_t getMicros()

    Get the current time in microseconds

.. c:function:: uint32_t getUsbStatus()

    Get the USB connection status. The return value is a bit field,
    as described below:

        * Bit 1 (1): Is there a cabled plugged into the brain?
        * Bit 2 (2): Is there an established connection from the
          brain cable to a host computer?
        * Bit 3 (4): Is there a cabled plugged into the controller?
        * Bit 4 (8): Is there an established connection from the
          controller cable to a host computer?

    Bits 5 (16) and 6 (32) are possibly used like bit 3 and 4 but for
    the partner controller, however I lack a second controller. It's
    worth noting that a connection from the controller **and** the
    brain at the same time is possible.

Controllers
-----------

.. c:function:: int32_t controllerGet(ControllerId id, ControllerChan channel)

    Query a "channel" from a controller. This usually just means
    getting a joystick value or a button, but you can also use it to
    get the current battery level of the controller, the power button
    in the middle of the controller, etc.

    :param id: The controller to query. Either :c:data:`Master` or
               :c:data:`Partner`.
    :param channel: The controller channel to query. This is one of:

        .. hlist::
            :columns: 3

            * :c:data:`LeftX`
            * :c:data:`LeftY`
            * :c:data:`RightX`
            * :c:data:`RightY`
            * :c:data:`BtnL1`
            * :c:data:`BtnL2`
            * :c:data:`BtnR1`
            * :c:data:`BtnR2`
            * :c:data:`BtnUp`
            * :c:data:`BtnDown`
            * :c:data:`BtnLeft`
            * :c:data:`BtnRight`
            * :c:data:`BtnX`
            * :c:data:`BtnY`
            * :c:data:`BtnA`
            * :c:data:`BtnB`
            * :c:data:`BtnSel`
            * :c:data:`BattLevel`
            * :c:data:`BtnAll`
            * :c:data:`Flags`
            * :c:data:`BattCapacity`

.. c:function:: ControllerStatus controllerStatus(ControllerId id)

    Get the status of a controller. The return value is either:

        * :c:data:`Offline`
        * :c:data:`Tethered`
        * :c:data:`VEXnet`

    This function does not differentaite between a VEXnet connection,
    or a Bluetooth connection from the robot radio.

    :param id: The controller to query. Either :c:data:`Master` or
               :c:data:`Partner`.

.. c:function:: bool setControllerText(ControllerId id, uint32_t line, uint32_t col, const char *str)

    Set a line of text on the controller. The return value indicates
    success.

    .. warning::
        This function must never be called more than once every 50ms
        due to limitations in how the controller works.

    :param id: The controller to query. Either :c:data:`Master` or
               :c:data:`Partner`.
    :param line: The line on the controller to set.
    :param col: The column to start writing at.
    :param str: A pointer to a char array, containing the string.

.. c:function:: bool clearControllerLine(ControllerId id, uint8_t line)

    This is a helper function to fill a line of text on the
    controller with spaces, effectively clearing that line.

    :param id: The controller to query. Either :c:data:`Master` or
               :c:data:`Partner`.
    :param line: The line on the controller to clear.

.. c:function:: bool controllerRumble(ControllerId id, const char *pattern)

    Make the controller  V I B R A T E!!

    :param id: The controller to query. Either :c:data:`Master` or
               :c:data:`Partner`.
    :param pattern: A pointer to a string containing the vibration
                    pattern.


Motors
------

.. note::
    All of these functions assume that a motor has been plugged into
    the specified port. If something other than a motor is plugged in
    unexpected magic™ might happed!

.. c:function:: void velocitySet(uint32_t port, int32_t velocity)

    Set the velocity of a motor. This will reset the motor's internal
    PID tracking, and is probably what you want.

    :param port: The port from 1-21 for the motor
    :param velocity: The velocity to set the motor to

.. c:function:: void velocityUpdate(uint32_t port, int32_t velocity)

    Update the target velocity for a motor's internal PID without
    resetting the state.

    :param port: The port from 1-21 for the motor
    :param velocity: The velocity to update the motor to

.. c:function:: void voltageSet(uint32_t port, int32_t voltage)

    Set the voltage to a motor. **When using this function, the
    internal PID for the motor will be ignored.**

    :param port: The port from 1-21 for the motor
    :param voltage: The velocity to send to the motor (in mV)

.. c:function:: int32_t velocityGet(uint32_t port)

    Get the current velocity that the motor is attempting to reach.

    :param port: The port from 1-21 for the motor

.. c:function:: int32_t directionGet(uint32_t port)

    Get the current direction of travel for the given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: int32_t velocityGetReal(uint32_t port)

    Get the real velocity that a given motor is running at.

    :param port: The port from 1-21 for the motor

.. c:function:: void pwmSet(uint32_t port, int32_t duty)

    Control a motor using a PWM duty cycle instead of voltage or
    velocity.

    :param port: The port from 1-21 for the motor
    :param duty: The duty cycle the motor should be run at.

.. c:function:: int32_t pwmGet(uint32_t port)

    Get the current PWM duty cycle that a motors is being driven at.

    :param port: The port from 1-21 for the motor

.. c:function:: void currentLimitSet(uint32_t port, int32_t limit)

    Set the current draw limit for a motor.

    :param port: The port from 1-21 for the motor
    :param limit: The limit that should be set for the motor.

.. c:function:: int32_t currentLimitGet(uint32_t port)

    Get the current draw limit for a motor.

    :param port: The port from 1-21 for the motor

.. c:function:: void voltageLimitSet(uint32_t port, int32_t limit)

    Set the voltage limit for a motor.

    :param port: The port from 1-21 for the motor
    :param limit: The voltage that should be set for the motor.

.. c:function:: int32_t voltageLimitGet(uint32_t port)

    Get the voltage limit for a motor.

    :param port: The port from 1-21 for the motor

.. c:function:: void setEncoderUnits(uint32_t port, EncoderUnits units)

    Set the units that should be reported by :c:func:`getMotorPos`.

    :param port: The port from 1-21 for the motor
    :param units: The units to use. One of:

        * :c:data:`Degrees`
        * :c:data:`Rotations`
        * :c:data:`Counts`

.. c:function:: EncoderUnits getEncoderUnits(uint32_t port)

    Get the units that are being reported by :c:func:`getMotorPos`.
    See also :c:func:`setEncoderUnits`.

    :param port: The port from 1-21 for the motor

.. c:function:: void setBrake(uint32_t port, BrakeMode mode)

    Set the brake mode that the motor will use when stopping.

    .. note::

        The brake mode is not respected when setting the voltage
        directly instead of using velocity control.

    :param port: The port from 1-21 for the motor
    :param mode: The brake mode to set the motor to. One of:

        * :c:data:`BrakeCoast`
        * :c:data:`BrakeBrake`
        * :c:data:`BrakeHold`

.. c:function:: BrakeMode getBrake(uint32_t port)

    Get the brake mode that the motor is currently using.

    :param port: The port from 1-21 for the motor

.. c:function:: void setMotorPos(uint32_t port, double position)

    Set the position of a given motor.

    :param port: The port from 1-21 for the motor
    :param position: The position the motor should be set to

.. c:function:: double getMotorPos(uint32_t port)

    Get the position of a given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: void resetMotorPos(uint32_t port)

    Reset the position counter of a given motor to 0.

    :param port: The port from 1-21 for the motor

.. c:function:: double getTarget(uint32_t port)

    Get the target that the motor PID is trying to achieve.

    :param port: The port from 1-21 for the motor

.. c:function:: void setServo(uint32_t port, double position)

    Set the position of a given motor, as if it were acting as a
    servo instead of a continuous motor.

    :param port: The port from 1-21 for the motor
    :param position: The postition that the motor should seek to

.. c:function:: void targetSetAbs(uint32_t port, double position, int32_t velocity)

    Set the absolute target of a motor.

    :param port: The port from 1-21 for the motor
    :param position: The absolute position that the motor should
                     target.
    :param velocity: The velocity at which the motor should move to
                     that target.

.. c:function:: void targetSetRel(uint32_t port, double position, int32_t velocity)

    Set the target of a motor, relative to its current location.

    :param port: The port from 1-21 for the motor
    :param position: The relative position that the motor should
                     target.
    :param velocity: The velocity at which the motor should move to
                     that target.

.. c:function:: void setGears(uint32_t port, Gearset gears)

    Set the gear ration that the motor is using, used for when trying
    to control the motor in terms of rotations or velocity. It is a
    good idea to reset the known gearing of every motor using this
    function in the :c:func:`setup` or :c:func:`init` function of
    user code.

    :param port: The port from 1-21 for the motor
    :param gears: The gearset to set the motor to. One of:

        * :c:data:`Gears36`
        * :c:data:`Gears18`
        * :c:data:`Gears06`

.. c:function:: Gearset getGears(uint32_t port)

    Get the current gearset that a given motor is set to. This might
    not be the actual gearset installed in the motor! This is just
    the gearset that has been set at some point previously.

    :param port: The port from 1-21 for the motor


Motor Reporting
---------------

The following functions report some form of status for a motor. They
can be useful when diagnosing problems, or maybe you could use them
in your `workflow <https://xkcd.com/1172/>`_.

.. c:function:: int32_t motorCurrent(uint32_t port)

    Get the current that a motor is currently drawing.

    :param port: The port from 1-21 for the motor

.. c:function:: int32_t motorVoltage(uint32_t port)

    Get the current voltage of a given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: double motorPower(uint32_t port)

    Get the current power for a given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: double motorEfficiency(uint32_t port)

    Get the current efficiency for a given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: double motorTemp(uint32_t port)

    Get the current temperature for a given motor.

    :param port: The port from 1-21 for the motor

.. c:function:: bool isOverTemp(uint32_t port)

    Checks if a motor is exceeding its safe operating temperature. If
    a motor is doing this, you might want to cool it down :).

    :param port: The port from 1-21 for the motor

.. c:function:: bool isOverCurrent(uint32_t port)

    Checks if a motor is drawing too much current. If this happens,
    you could increase the maximum, or check if there's a mechanical
    fault jamming the mechanism.

    :param port: The port from 1-21 for the motor

.. c:function:: uint32_t getFaults(uint32_t port)

    Get faults for a particular motor. Your motors shouldn't usually
    fault, so if they do that's probably bad.

    :param port: The port from 1-21 for the motor

ADI Interface
-------------

Genric ADI Devices
~~~~~~~~~~~~~~~~~~

.. c:function:: void setupAdi(uint32_t port, AdiType type)

    Set the type of device connected to a 3-wire port

    :param port: The 3-wire from 'a'-'h' (or 1-8)
    :param type: The type of device connected. One of:

        .. hlist::
            :columns: 2

            * :c:data:`AnalogIn`
            * :c:data:`AnalogOut`
            * :c:data:`DigitalIn`
            * :c:data:`DigitalOut`
            * :c:data:`SmartButton`
            * :c:data:`SmartPot`
            * :c:data:`LegacyButton`
            * :c:data:`LegacyPotentiometer`
            * :c:data:`LegacyLineSensor`
            * :c:data:`LegacyLightSensor`
            * :c:data:`LegacyGyro`
            * :c:data:`LegacyAccelerometer`
            * :c:data:`LegacyServo`
            * :c:data:`LegacyPwm`
            * :c:data:`QuadEncoder`
            * :c:data:`Sonar`
            * :c:data:`LegacyPwmSlew`

.. c:function:: void adiSet(uint32_t port, int32_t value)

    Set the output value of a 3-write port. What this does is
    dependant on how :c:func:`setupAdi` was used.

    :param port: The 3-wire from 'a'-'h' (or 1-8)
    :param value: The value to set the port to.

.. c:function:: void adiGet(uint32_t port)

    Reads a value from the 3-wire interface. As with
    :c:func:`adiSet`, this is heavily dependant on
    :c:func:`setupAdi`.

    :param port: The 3-wire from 'a'-'h' (or 1-8)

Ultrasonics
~~~~~~~~~~~

.. c:function:: ultra_t setupUltra(uint32_t ping, uint32_t echo)

    Sets up an ultrasonic (sonar) sensor on two ports.

    .. note::

        These two ports must be either AB, CD, EF or GH. Any other
        combination will fail to work.

    :param ping: The 3-wire port the ping wire is in
    :param echo: The 3-wire port the echo wire is in

.. c:function:: uint32_t ultraGet(ultra_t ultra)

    Read the value from an ultrasonic sensor.

    :param ultra: An ultrasonic sensor, as returned by
                  :c:func:`setupUltra`.

.. c:function:: void ultraStop(ultra_t ultra)

    Stop using a pair of ports as an ultrasonic sensor.

    :param ultra: An ultrasonic sensor, as returned by
                  :c:func:`setupUltra`.

Robot Battery
-------------

If, for some reason, you are powering your robot brain through means
other than the official Robot Battery, these functions will return 0,
however this is undefined behaviour and should not be relied on.

.. c:function:: int32_t batteryVoltage()

    Get the current voltage of the connected robot battery.

.. c:function:: int32_t batteryCurrent()

    Get the current current draw on the connected robot battery from
    the brain.

.. c:function:: double betteryTemp()

    Get the temperature of the connected robot battery.

.. c:function:: double betteryCapacity()

    Get the capacity of the connected robot battery.

Graphics
--------

.. c:function:: void foregroundColor(uint32_t col)

    Set the foreground colour to use in the following drawing
    functions.

    :param col: The colour to use.

.. c:function:: void backgroundColor(uint32_t col)

    Set the background colour to use in the following drawing
    functions.

    :param col: The colour to use.

.. c:function:: void clearDisplay()

    Clear the display

.. c:function:: void printfDisplay(int32_t xpos, int32_t ypos, uint32_t opacity, const char *format, ...)

    Put text onto the display. See also :c:func:`setFont`.

    :param xpos: The X position on the display
    :param ypos: The Y position on the display
    :param opacity: The opacity of the text being rendered
    :param format: A printf-style format string

.. c:function:: void drawLine(int32_t x1, int32_t y1, int32_t x2, int32_t y2)

    Draw a line onto the display.

    :param x1: The starting X coordinate
    :param y1: The starting Y coordinate
    :param x2: The ending X coordinate
    :param y2: The ending Y coordinate

.. c:function:: void drawRect(int32_t x1, int32_t y1, int32_t x2, int32_t y2)

    Draw a rectangle's outline onto the display.

    :param x1: The X coordinate of the top left
    :param y1: The Y coordinate of the top left
    :param x2: The X coordinate of the bottom right
    :param y2: The Y coordinate of the bottom right

.. c:function:: void fillRect(int32_t x1, int32_t y1, int32_t x2, int32_t y2)

    Draw a filled rectangle onto the display.

    :param x1: The X coordinate of the top left
    :param y1: The Y coordinate of the top left
    :param x2: The X coordinate of the bottom right
    :param y2: The Y coordinate of the bottom right

.. c:function:: void drawCircle(int32_t xc, int32_t yc, int32_t radius)

    Draw the outline of a circle onto the display.

    :param xc: The X coordinate of the centre
    :param yc: The Y coordinate of the centre
    :param radius: The radius of the circle to draw

.. c:function:: void fillCircle(int32_t xc, int32_t yc, int32_t radius)

    Draw a filled circle onto the display.

    :param xc: The X coordinate of the centre
    :param yc: The Y coordinate of the centre
    :param radius: The radius of the circle to draw

.. c:function:: void setAt(uint32_t x, uint32_t y)

    Set a single pixel on the display.

    :param x: The X coordinate
    :param y: The Y coordinate

.. c:function:: bool setFont(FontFace face)

    Set the font that should be used when putting text onto the
    display.

    :param face: The font face to use. One of:

        * :c:data:`Monospace`
        * :c:data:`Proportional`
