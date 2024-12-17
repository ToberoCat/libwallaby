# Guide to Troubleshooting and Preventing Servo Port Failures on Wombat Controllers

<primary-label ref="kit"/>
<secondary-label ref="actuator"/>
<secondary-label ref="tobias-madlberger"/>

## Understanding the Problem

Servo ports on Wombat controllers can fail, resulting in reduced or no power being delivered to connected servos. Common
symptoms include:

- **Servos failing to move** or stopping mid-operation.
- **Servos entering a failsafe state**, requiring manual reset.
- Some ports working while others do not.

Even unused ports have been observed to fail. This guide explores potential causes and practical solutions.

## Common Causes of Servo Port Failures

1. **Excessive Power Draw**: Servos under heavy load draw more current, potentially tripping fuses that protect the
   controller.
2. **Repeated Fuse Tripping**: Over time, internal resistance increases, degrading performance.
3. **Servo Torque Exceeding Limits**: If a servo attempts to move or hold weight beyond its capacity, it strains both
   the servo and controller port.
4. **Improper Power Supply**: Voltage irregularities or unstable power can damage ports.
5. **Mechanical Stress**: Improper mounting or excessive tension on servos may cause instability and overdraw.

## Diagnosing the Issue

1. **Test the Servos**:
    - Plug suspect servos into known working ports on another Wombat controller.
    - Use new servos in suspected faulty ports to confirm behavior.

2. **Inspect Fuses**:
    - Look for small fuses (likely surface-mounted) protecting each servo port.

3. **Check for Overloading**:
    - Calculate the force and torque required (explained in Section 7). Compare it to the servo’s datasheet
      specifications.

4. **Power Supply Check**:
    - Ensure stable voltage from the power supply. Use a multimeter to test output.

## Technical Explanation: Fuses and Power Overload

The Wombat controller includes **self-resetting fuses** to protect servo ports from excessive current draw.

- **Why Fuses Trip**: When a servo encounters excessive load (e.g., lifting too much weight), it draws too much current,
  causing the fuse to trip.
- **Degradation Over Time**: Repeated tripping increases internal resistance in the fuse, reducing power to the port and
  eventually making it unusable.

### Identifying Burned Ports

If a port delivers little to no power but hasn’t been permanently destroyed, it’s likely due to accumulated fuse
resistance.

## Solutions for Burned-Out Ports

1. **Replace the Fuse**
    - Identify the fuse part number (check KIPR’s GitHub or Wombat schematics, but it seems like it's the fuse **0603l100slyr**).
    - Purchase compatible surface-mounted fuses.
    - Carefully desolder the old fuse and solder a replacement.

2. **Workaround**: Use alternate ports or redistribute servo load across multiple controllers.

## Preventive Measures

1. **Avoid Overloading Servos**:
    - Calculate torque and force requirements (Section 7).
    - Use servos rated for higher torque if necessary.

2. **Reduce Load on Servos**:
    - Add counterbalances or rubber bands to reduce mechanical strain.
    - Replace heavy components with lightweight materials (e.g., LEGO, 3D-printed parts).

3. **Use Multiple Servos**:
    - Distribute load across two or more servos when possible.

4. **Monitor Voltage and Current**:
    - Use power regulation to maintain a stable voltage supply.

5. **Optimize Mechanical Design**:
    - Minimize unnecessary forces acting on the servo. Ensure smooth and precise movements.

## How to Calculate Force and Torque Requirements

To avoid overloading servos, calculate the torque and force required:

### **Torque Calculation**

The torque T is given by:
$$
T = F \cdot d
$$
Where:

- T = Torque (Nm)
- F = Force applied (N)
- d = Distance from pivot point (m)

1. **Find Center of Mass**: Determine where the load acts relative to the servo arm’s pivot point.
2. **Calculate Force**: Use the formula $$ F = m \cdot g $$, where  m is mass (kg) and g is gravity (~9.81
   m/s²).
3. **Compare Torque**: Check the torque value against the servo’s datasheet rating.

### **Practical Example**

- A servo arm 10 cm long (d = 0.1 m) holding a 500 g object (m = 0.5 kg):  
  $$
  F = 0.5 \cdot 9.81 = 4.905 \, N  
  T = 4.905 \cdot 0.1 = 0.4905 \, Nm
  $$

Ensure the servo’s rated torque exceeds this value.

## Motors are modified Servos

- Motors in Wombat kits may be modified servos (e.g., MG996R). Treat their specifications similarly.

## Additional Resources

- **KIPR Documentation**: [KIPR GitHub Docs](https://github.com/kipr/KIPR-Development-Toolkit/tree/master/Docs)
- **Servo Datasheets**: <resource src="mg996r.pdf">MG996r</resource>
- **Troubleshooting Forums**: KIPR forums, robotics communities like Reddit, or FIRST Robotics groups.

### Conclusion

Servo ports burning out is typically caused by excessive current draw, often due to mechanical stress or torque
overload. By calculating force requirements, replacing damaged fuses, and optimizing mechanical design, teams can
minimize or prevent failures.

This guide provides a systematic approach to troubleshooting, repairing, and improving servo usage, ensuring reliable
performance in competition settings.

<include from="snippet-lib.md" element-id="footer">
    <var name="authors">Tobias Madlberger</var>
</include>