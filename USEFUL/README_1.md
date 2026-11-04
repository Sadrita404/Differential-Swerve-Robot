# Differential Swerve Robot

*This README was compiled with AI from my spoken notes.*

A four-module differential swerve robot with a turret, almost entirely 3D printed and driven by NEMA 17 steppers.

The mechanical parts cost about $110 to $150. A commercial FRC swerve module costs $500 or more with motors, and that is for one module. With breadboard electronics, the whole robot is under $200.

## Cost

| Build | Cost |
|---|---|
| Mechanical parts | $110 to $160 |
| Electronics, breadboard wiring | $57 to $66 |
| Total, breadboard build | $167 to $224 |

These are Amazon prices. The low end uses 38mm motors and 48 U-groove bearings. The high end uses 48mm motors, 64 bearings and better bolts. Filament, the optional turret bearing, the optional slip ring and the optional cover are not included. The full BOM is in `swerve_robot_bom.xlsx`.

## Printing

Printing everything takes a few days and a few rolls of filament.

The wheels are PLA. TPU should also work, and you can design your own wheel. Any wheel has to touch the ground in a line contact at the center of the wheel. If the contact point moves off center, the control code gets screwed up.

## Motors

The drive uses eight NEMA 17 steppers, two per module. I used 48mm motors, which have more torque than the robot needs. 38mm motors should also work. 23mm pancake NEMA 17s do not work because they are not strong enough.

## Stepper drivers

A4988, DRV8825 and TMC2209 stepsticks all work.

- **TMC2209** is the best of the three and is quiet. My PCB is laid out for A4988s, so TMC2209 UART control is not possible without big changes to the board. The drivers still run in step/dir mode.
- **A4988** is loud and buzzes, but you can play music with it. This is why my PCB was designed for A4988s.
- **DRV8825** shut off on their own in my experience. I don't recommend them.

## Electronics

You can wire the drivers on a breadboard or use a driver PCB. Breadboard wiring is harder but much cheaper.

My PCB is a 4-layer, 12-channel stepper driver board (PCBWay sponsored mine). I designed it as a general-purpose board for future projects, so it is bigger than this robot needs and the middle is empty. Only 9 channels are used here. There is room in the middle to add an MCU.

### Microcontroller

I used an ESP32. Any MCU with about 20 GPIO pins works.

- 16 pins for the 8 drive motors (step and dir each)
- 2 pins for the turret motor
- 1 pin for the global enable on the PCB, which saves a lot of power when the motors are idle

That is 19 pins. A second turret stepper adds 2 more, for 21.

### Battery

With TMC2209s and 48mm steppers, peak current draw is about 2A. A 6S 5Ah Li-ion pack is more than enough. These are safer than LiPos and on Amazon they usually come with a BMS and charger. They are more cube-shaped than a LiPo, which is lower profile.

The MCU can run from a USB power bank. If you want to run it from the main battery, add a 5V buck converter.

## Assembly tips

### U-groove bearing spacing

Each module uses 12 to 16 U-groove bearings, so the robot needs 48 to 64 in total. 8 bearings per module hold the ring gears. Another 4 to 8 per module mount the module in the chassis.

The most important dimension is the spacing of the U-groove bearings, both inside each module and in the wheel frame on the chassis.

- If the spacing is too small, the bearings don't hug the groove on the ring. The ring is too loose and the gears skip all the time.
- If the spacing is too big, there is too much friction. In the worst case you can't mount the ring at all.

The torus bead on the ring has a smaller radius than the groove in the bearing. As the robot breaks in, the bead wears to match the groove and the motion gets smoother.

If your spacing is off, use the included spacing jig to find the right value. The jig steps in 0.1mm increments. Then edit the STEP file to set that spacing and re-export the part.

### Gear skipping

If the modules skip under hard acceleration, fill more of the 8 mounting bearing slots around each module. More bearings hold the module tighter but also add friction.

## Turret

Turret assembly instructions are not included because the wiring depends on whether you use the PCB or a breadboard. The turret is fairly simple to put together. It lifts straight off, so you can reach the electronics underneath.

The turret uses one stepper by default. If you add a second turret stepper, you also need a slip ring so the wires can pass through the rotating joint. A 10-wire slip ring costs about $20, and the adapter plate and turret gear are designed to fit one.

The turret bearing can be 3D printed, and the file is included. You can also buy an equivalent bearing with a 100mm inner diameter and 125mm outer diameter for about $20. This is optional.

## Cover

The cover is optional and only cosmetic. SendCutSend quoted about $125 to laser cut it from polycarbonate (mine was sponsored). If you have access to a laser cutter, it costs only the material.

You can also 3D print the cover as four segments that lock together. The downside is that it hides the mechanism, which is the interesting part of the robot.

## Known limitations

- Steppers limit top speed and can lose position without encoders.
- The U-groove bearing ring mount has some play.
- The gears can skip at high speed.
- The design works best at low speed. A heavier-duty version would need large ring bearings and encoders.

## Credits

- James Lin reworked the V2 CAD around sideways 48mm motors and bevel gears.
- The Standard Manufacturing Company sponsored the robot. 
- LDO Motors sponsored the stepper motors.
- PCBWay sponsored the driver PCB.
- SendCutSent sponsored the transparent cover.
