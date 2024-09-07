# IMUs in Botball

<secondary-label ref="sensor"/>
<secondary-label ref="tobias-madlberger"/>

This guide will introduce you to the basics of using an Inertial Measurement Unit (IMU) in your robotics projects.
You’ll learn how to estimate your robot's orientation using gyroscope data and how to use this information to make your
robot rotate to a specific angle and drive straight with improved accuracy.

The methods described here are a good starting point but are not perfect. They can be improved with techniques like
sensor fusion, PID controllers, or other advanced methods. By the end of this guide, you should have a solid
understanding of how to start using an IMU in your robotics projects.

## What is an IMU?

An IMU, or Inertial Measurement Unit, is a sensor that typically includes a gyroscope, an accelerometer, and sometimes a
magnetometer.

### Key Components of an IMU

1. **Gyroscope**: Measures the rate of rotation around the x, y, and z axes. It provides data in degrees per second (
   dps) and is crucial for determining the robot’s orientation.

2. **Accelerometer**: Measures the linear acceleration along the x, y, and z axes. This can help determine the robot's
   speed and tilt by detecting changes in velocity and orientation.

3. **Magnetometer**: Measures the strength and direction of the magnetic field around the sensor, often used to
   determine the robot's heading relative to the Earth's magnetic field (like a compass).

### Real-World Applications

IMUs are widely used in various technologies:

- **Smartphones**: For detecting screen orientation, motion tracking in AR/VR applications, and step counting.
- **Drones**: To stabilize flight by constantly adjusting orientation and speed.
- **Wearables**: For fitness tracking, detecting movement, and monitoring posture.

### Importance of Proper IMU Placement

To achieve accurate measurements, it's essential to mount the IMU correctly on your robot:

1. **Orientation**: The IMU should be mounted at a 90-degree angle relative to the robot’s frame, with one of its axes (
   x, y, or z) pointing directly upwards.

2. **Stability**: The IMU must be securely attached to the robot to prevent any movement that could affect the sensor
   readings.

3. **Placement**: Place the IMU as close to the center of the robot as possible to minimize the effects of the robot’s
   movements on the IMU data.

## Understanding Gyroscope Data

The gyroscope in the IMU measures angular velocity, which is the rate of rotation around the three axes. This data is
usually given in degrees per second (dps). By integrating this angular velocity over time, you can estimate the robot’s
orientation.

### Basic Gyro Integration

To estimate the robot’s orientation, you can integrate the gyroscope data over time. The basic idea is to add the
product of the angular velocity and the time interval to the current orientation.

Here’s a simple example in pseudocode with added comments for clarity:

```Javascript
function estimate_orientation():
    last_time = get_current_time()  // Store the initial time
    orientation = 0  // Initialize the orientation to 0
    while True:
        gyro_x, gyro_y, gyro_z = read_gyroscope()  // Read gyroscope data
        current_time = get_current_time()  // Get the current time
        delta_time = current_time - last_time  // Calculate the time difference
        orientation = orientation + gyro_z * delta_time  // Update the orientation based on angular velocity
        last_time = current_time  // Update last_time to current time
        print("Estimated orientation:", orientation)  // Output the estimated orientation
```

### Understanding Gyro Drift

Over time, small errors in the gyroscope data can accumulate, leading to inaccuracies in the estimated orientation. This
phenomenon is known as **gyro drift**. Gyro drift occurs due to factors like sensor noise, temperature changes, and
imperfect calibration. While it might not be noticeable in short operations, it can become significant over extended
periods.

Here's a guide about the two main noise types within a
gyro: [Gyro Noise Analysis](https://mwrona.com/posts/gyro-noise-analysis/)

![Gyro Drift](gyro_drift.png)

*This graph shows the three estimated angles slowly drifting away when the robot doesn't move and sits on a flat
surface*

## Calibrating the Gyroscope

To improve accuracy, you need to calibrate the gyroscope to remove any bias or offset in the sensor readings.
Calibration involves taking multiple readings while the robot is stationary and calculating an average value to
determine the bias. This bias is then subtracted from the gyroscope data during operation.

Here’s a basic calibration example with enhanced explanations:

```Javascript
function calibrate_gyroscope():
    gyro_x_sum = 0
    gyro_y_sum = 0
    gyro_z_sum = 0
    num_samples = 100  // Number of samples for averaging
    for i in range(num_samples):
        gyro_x, gyro_y, gyro_z = read_gyroscope()  // Read gyroscope data
        gyro_x_sum += gyro_x  // Accumulate gyro x readings
        gyro_y_sum += gyro_y  // Accumulate gyro y readings
        gyro_z_sum += gyro_z  // Accumulate gyro z readings
    gyro_x_bias = gyro_x_sum / num_samples  // Calculate bias for gyro x
    gyro_y_bias = gyro_y_sum / num_samples  // Calculate bias for gyro y
    gyro_z_bias = gyro_z_sum / num_samples  // Calculate bias for gyro z
    print("Gyroscope biases:", gyro_x_bias, gyro_y_bias, gyro_z_bias)
```

### Using Built-In Calibration Functions

In some libraries, like the KIPR library for the Wombat robot, a calibration function might already be provided, such
as `gyro_calibrate()`. You can use this function to simplify the calibration process.

## Using the Gyroscope to Rotate Your Robot

Now that you understand how to estimate orientation using gyroscope data, you can use this knowledge to make your robot
turn by a specific angle. Here’s an improved method for turning your robot 90 degrees:

```Javascript
function turn_90_degrees():
    gyro_calibrate()  // Calibrate the gyroscope
    target_angle = 90  // Set target angle to 90 degrees
    current_angle = 0  // Initialize current angle to 0
    last_time = get_current_time()  // Store the initial time
    while current_angle < target_angle:
        gyro_z = read_gyroscope()  // Read gyroscope z-axis data
        current_time = get_current_time()  // Get the current time
        delta_time = current_time - last_time  // Calculate the time difference
        current_angle += gyro_z * delta_time  // Update the current angle
        last_time = current_time  // Update last_time to current time
        
        if current_angle >= target_angle:  // Check if the target angle is reached
            stop_robot()  // Stop the robot
        else:
            turn_robot()  // Continue turning the robot
```

### Error Handling: What If the Gyroscope Fails?

During testing, you might encounter situations where the robot does not turn the expected amount, even when the code
seems correct. This could be due to faulty gyroscope data, which can occur if the gyroscope is damaged or miscalibrated.

Here’s how you can handle potential errors:

```Javascript
function turn_90_degrees_with_error_handling():
    try:
        gyro_calibrate()
        target_angle = 90
        current_angle = 0
        last_time = get_current_time()
        while current_angle < target_angle:
            gyro_z = read_gyroscope()
            current_time = get_current_time()
            delta_time = current_time - last_time
            current_angle += gyro_z * delta_time
            last_time = current_time
            
            if current_angle >= target_angle:
                stop_robot()
            else:
                turn_robot()
    except GyroscopeError:
        print("Error: Gyroscope malfunction detected!")
        stop_robot()
        # Additional error recovery code could be added here
```

In this example, you can implement a custom error class (`GyroscopeError`) to handle specific gyroscope-related issues,
such as out-of-range values or communication errors with the IMU.

### Compensating for Faulty Gyroscope Data

If you notice consistent errors in the robot's turns, you might need to apply a correction factor:

```Javascript
corrected_target_angle = target_angle * correction_factor
```

In a real-world scenario, this factor is determined experimentally. For instance, if your robot only turns 30 degrees
when it should turn 90, you would need to multiply the target angle by 3 to achieve the correct turn.

## Improving Accuracy with PID Control

To achieve more accurate turns, you can use
a [PID controller](https://en.wikipedia.org/wiki/Proportional%E2%80%93integral%E2%80%93derivative_controller) to adjust
the motor speeds based on the error between the
target angle and the current angle. 
To easier understand the PID in a robotics related context, one can look at the following video:
[PID - Controlling Self Driving Cars](https://www.youtube.com/watch?v=4Y7zG48uHRo)

Here’s a more refined example of how you might implement this:

```Javascript
function turn_robot():
    left_motor_port = 0
    right_motor_port = 1
    error = target_angle - current_angle  // Calculate the error
    kP = 1.0  // Proportional gain
    left_motor_speed = kP * error  // Adjust left motor speed
    right_motor_speed = -kP * error  // Adjust right motor speed
    mav(left_motor_port, left_motor_speed)  // Move left motor
    mav(right_motor_port, right_motor_speed)  // Move right motor
```

In this simplified example, only the proportional term (`kP`) is used, but you can expand this by adding integral (`kI`)
and derivative (`kD`) terms for better control.

## Driving Straight with the Gyroscope

The same principles used for turning can be applied to driving straight. By

maintaining a constant orientation (e.g., 0 degrees), you can adjust the motor speeds to correct any deviations:

```Javascript
function drive_straight():
    gyro_calibrate()
    target_angle = 0  // Set the target angle to 0 (straight)
    current_angle = 0  // Initialize current angle to 0
    last_time = get_current_time()  // Store the initial time
    while True:  // or until a certain condition is met
        gyro_z = read_gyroscope()  // Read gyroscope data
        current_time = get_current_time()  // Get the current time
        delta_time = current_time - last_time  // Calculate the time difference
        current_angle += gyro_z * delta_time  // Update the current angle
        last_time = current_time  // Update last_time to current time
        error = target_angle - current_angle  // Calculate the error
        adjust_speed(error)  // Adjust motor speeds based on the error

function adjust_speed(error):
    base_speed = 50  // Base speed for both motors
    left_motor_port = 0
    right_motor_port = 1
    kP = 1.0  // Proportional gain for correction
    left_motor_speed = base_speed + kP * error  // Adjust left motor speed
    right_motor_speed = base_speed - kP * error  // Adjust right motor speed
    mav(left_motor_port, left_motor_speed)  // Move left motor
    mav(right_motor_port, right_motor_speed)  // Move right motor
```

![Bang Bang Control Driving Straight](bang-bang-control-driving-straight.gif)

*In the GIF, one can see how this implementation would be driving straight. 
One can also see the issues the basic error correction (Bang-Bang Control) has*

Again, this method is a basic implementation and might not work perfectly over long distances due to gyro drift.

## Further Enhancements

While the methods discussed are a good starting point, they can be enhanced in several ways:

- **Sensor Fusion**: Combine gyroscope data with accelerometer and magnetometer data to reduce drift using algorithms
  like the Complementary Filter or Kalman Filter.
- **PID Controller**: Use a PID controller to fine-tune motor speeds and improve accuracy in both turning and straight
  driving.
- **Adaptive Calibration**: Continuously recalibrate the gyroscope to account for changes in environmental conditions
  that could affect sensor accuracy.
- **Landmark-based Error Correction**: Use known environmental features, like lines on the floor or walls, to correct
  orientation drift.
- **Dead Reckoning with Error Estimation**: Combine gyroscope data with other sensors like wheel encoders to cross-check
  and reduce drift.

## Conclusion

Using an IMU in your robotics projects can significantly improve your robot’s ability to rotate and drive straight.
However, it's essential to understand the limitations of the gyroscope, particularly drift, and explore ways to mitigate
it.

If you encounter issues with inaccurate turns, consider experimenting with correction factors as described in this
guide. By starting with the basic methods provided here and then exploring more advanced techniques, you can develop a
highly accurate system for controlling your robot’s movements.

While the IMU is a powerful tool, it should not be relied upon exclusively. I strongly recommend combining it with other
methods, such as driving against PVC pipes or aligning with black lines, to achieve the best accuracy.

For more insights on the pros and cons of different approaches and additional techniques to enhance your robot’s
accuracy, refer to the increasing robot accuracy guide.

<include from="snippet-lib.md" element-id="footer">
    <var name="authors">Tobias Madlberger</var>
</include>