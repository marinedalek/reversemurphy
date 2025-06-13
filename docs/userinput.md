## Mouse Input
(Note: This information needs checking against Under a Killing Moon. Currently based upon analysis of The Pandora Directive.)
Mouse motion is derived from calls to INT 0x33, AH = 0xb. This provides the motion counters for the x and y axes, which are scaled based upon the Mouse Sensitivity preference
setting. If the setting is Low, each motion counter is shifted right one bit (divided by 2), or if it is High they are shifted left one bit (multiplied by 2). Med sensitivity
leaves the motion counter values unchanged. These scaled values are then used to update two sets of variables. 
The first set is an absolute position, clamped by a screen-space bounding box. The second is a 2D delta, giving the amount of movement since the previous mouse read.


### Movement Mode
While in movement mode, the game reads the mouse every frame(?). The mouse delta variables are applied to an internally-modeled virtual joystick. The joystick's position is 
stored in two integers, which are each clamped to the range -255 to +255 after the corresponding mouse delta is added. A nonlinear function is then applied to the virtual joystick
values. This has the effect of decreasing mouse sensitivity at slow movement speeds, allowing finer control over precise manoeuvring. The following c snippet models how one of the 
joystick values is transformed.

```c
int absJoyY = abs(joyY);

int playerFwdWalk = lawLUT[absJoyY >> 0x4 & 0xf] * 0x2000;
if (joyY < 0) {
  playerFwdWalk = -playerFwdWalk;
}
```

In the above snippet, `joyY` is one of the clamped virtual joystick axes. The absolute value of this is shifted right four bits (divided by 16) and masked. It is then used as the
index into a 16-element lookup table, `lawLUT[]` that provides a piecewise linear approximation of an exponential curve. The looked-up value is then multiplied by `0x2000` and stored in 
`playerFwdWalk` Finally, the sign of the original `joyY` value is applied to the result. 

Note that, because downward mouse motion results in a positive Y counter, negative virtual joystick values are mapped to forward motion of Tex. 

For fore and aft motion, the Walking Speed[^walkspeed] is used to scale the resulting value. For Slow speed, the value is shifted right 2 bits (divided by 4), and for Medium speed, the value 
is shifted right 1 bit (divided by 2). Tex's lateral movement is always shifted right 2 bits; the same as Slow forward walking. 

Holding down `R` for Run temporarily sets Walking Speed to Fast.

Tex's motion is further scaled by a value that may be screen refreshes per rendered frame (more analysis needed). This compensates for variable framerates by moving Tex further 
if the last frame took longer to draw.


[^walkspeed]: Mislabeled as "Speed Walking" in the Preferences menu. Maybe Tex is trying to keep in shape?
