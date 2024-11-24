#MachineDesign 
## Da Problem 
1. The spinny pill boi needs to be able to stop right away when the pill chute reaches 30 pills.
2. The conveyor needs to be able to stop right away when the bottle arrives at the break beam sensor (without knocking bottles over). 
3. All this needs to be achieved with acceleration handled by Accelstepper

## Da Solution (To Problem 1)

#### The Issue
The core of this issue revolves around how to stop the motor right away when the sensor is triggered. This is tricky because in the most common way Accelstepper is used, the control of the motor is based on movements of set size. We use the `motor.move(NUM_STEPS)` method to make the motor move a relative number of steps from where it is now. This is useful because it means Accelstepper knows ahead of time what it's target location is, so it can compute the trajectory beforehand and start slowing down at the right time to reach zero velocity at the target location. 

However, this presents a problem when we try to drive it based on a sensor input that requires the motor to stop right away when the sensor triggers. Because the sensor trigger could come at any time, Accelstepper does not know its target stopping point and therefore cannot plan its deceleration. At a high level, the two ways around this are to either start decelerating when the sensor triggers, or to just skip deceleration entirely and just dead-stop the motor right when the sensor triggers (Basically and arbitrarily high deceleration). Sadly, both these methods have problems:

**Deceleration Method**

| Pros                                                                                                           | Cons                                                                        |
| -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| With a proper acceleration, no chance of damaging the system with excessive forces            | There will be an overshoot since it takes time to bring the motor to a stop |
| Motor position is controlled at all times, no chance of inertia causing it to go more steps than we ask |                                                                             |                                                                      |

**Dead Stop Method**

| Pros                                     | Cons                                                                                        |
| ---------------------------------------- | ------------------------------------------------------------------------------------------- |
| The motor stops right away, no overshoot | If the speed of the motor is too high, it will skip steps and overshoot                     |
|                                          | If the motor is moving fast, it can break things because of the force of the motor stopping |

Since no extra pills can be allowed to dispense, the deceleration method won't work. So, the best option is the dead stop method with a sufficiently low speed such that when the motor stops, the forces are not enough to cause it to slip and overshoot or cause damage to the system. 

#### Implementing the Dead-Stop
For the dead-stop method, we want to accelerate to a constant speed, then hold that constant speed until a trigger causes us to immediately stop the motor. 

**The Quick and Dirty Way**
A quick and dirty way to do this is as follows (Omitting things like variables motor and variable declarations and using some pseudo-code).

```C
void setup() {
	// The acceleration to be used when coming up to speed 
	myStepper.setAcceleration(ACCELERATION);
	
	// Choose a speed slow enough that the motor does not overshoot or break
	// anything when it jerks to a stop
	myStepper.setMaxSpeed(SPEED);
}

void loop() {
	myStepper.run(); // Always call this
	
	if(need_dispense_pills) {
		// Make the motor go to a really far spot, more than you would ever need 
		// to get 30 pills
		myStepper.move(1000000); 
	}

	if(pills_dispensed > 30) {
		// Set the position the motor is at right now to 0 to make the stepper 
		// think it has arrived and instantly go to zero velocity
		myStepper.setCurrentPosition(0);
	}
}
```
This implementation works by abusing the normal move command to achieve acceleration followed by a long period of constant speed movement. Then, when the sensor says 30 pills have come out, we set the current position and velocity to zero, stopping the motor right away (continued calls to `myStepper.run()` do nothing because the motor is already at the 0 position). 

**NOTE:** It is critical that no delays are used in the loop or all of this breaks down.

This method is quick to implement, but does have some weaknesses. First, if it takes longer than 1,000,000 steps to get 30 pills, the motor will reach the end of its travel and decelerate to a stop. Second, the speed needs to be low enough to avoid too much jerk when the motor stops, but this could also mean that it takes a super long time to get 30 pills out because the motor rotates slowly (How quick you can go depends on your system and has to be experimentally tuned). 

**A Band-Aid fix for the above issues**
Here is a Band-Aid fix solution for the above issues. Pretty hacky but it avoids most of the pitfalls.

```C
int flag_dispensing = 0;

void setup() {
	// The acceleration to be used when coming up to speed 
	myStepper.setAcceleration(ACCELERATION);
	
	// This initial speed no longer has to be super slow
	myStepper.setMaxSpeed(SPEED);
}

void loop() {
	myStepper.run(); // Always call this

	if(flag_dispensing == 1) {
		// If we are currently supposed to dispense keep adding more 
		// distance relative to the current motor location
		myStepper.move(1000);
	}

	if(command_recieved_to_dispense_pills) {
		//flag that we need to start dispensing pills
		flag_dispensing = 1;
	}

	if(pills_dispensed > 24) {
		// When the 25th pill is counted, decrease the speed to 
		// to a safe dead-stop speed
		myStepper.setMaxSpeed(DEAD_STOP_SPEED);
	}

	if(pills_dispensed > 29) {
		// When the 30th pill is counted, Set the position the motor is at 
		// right now to 0 to make the stepper think it has arrived 
		// and instantly go to zero velocity
		myStepper.setCurrentPosition(0);

		// Then flag that we are no longer dispensing
		flag_dispensing = 0;
	}
}
```
This implementation improves on the previous one in two ways. First, rather than just setting one super long move when dispensing starts (and potentially not dispensing 30 pills before the move ends), the dispensing/non-dispensing status is tracked with a flag and every time the loop comes around, if the system is supposed to be dispensing, a smaller move is appended. This way, the move will never run out. Second, to avoid the pitfall of needing a slow enough speed to handle the abrupt stop but also not wanting dispensing to take a million years, the motor speed is lowered after the 25th pill is counted. 

This method fixes the problems found in the quick and dirty implementation, however it does still have some inefficiencies and does things in a bit of a round about way. By taking advantage of some other features of the Accelstepper library and writing some more code, we can have a more robust and efficient implementation.

**The Better (and Fancier) Way to Do it**
This final implementation relies on a function in the Accelstepper library called `myStepper.runSpeed()` which allows us to run a constant speed with no target end point. It is used similar to `myStepper.run()` in that, after setting the desired speed with `myStepper.setSpeed(DESIRED_SPEED)`, repeated calls to `runSpeed()` will cause the motor to step at the desired speed. Sadly, `runSpeed()` does not accelerate up to the target speed and instead just goes full beans and pours all the speed on at once. If your target speed is slow, this is ok, but it is better to manually implement acceleration. To do this manual acceleration implementation, we need to introduce a finite state machine that keeps track of whether we are in the acceleration or constant speed movement phase.

**NOTE:** `setSpeed()` and `setMaxSpeed()` are two different things. `setSpeed()` sets the desired speed that `runSpeed()` will do, while `setMaxSpeed()` sets the maximum speed that the motor will accelerate to when commanded with `move()` and `run()`

```C
#define MOTOR_ACCELERATING 1
#define MOTOR_CONST_SPEED 2
#define DESIRE_SPEED 200 // Whatever the target speed is

int motor_state = MOTOR_ACCELERATING;

int flag_dispensing = 0;

void setup() {
	// The acceleration to be used when coming up to speed 
	myStepper.setAcceleration(ACCELERATION);
	
	myStepper.setMaxSpeed(SPEED);
}

void loop() {

	// If we are dispensing, do the stuff to make the motor move
	if(flag_dispensing == 1){
		// Switch case to implement the finite state machine
		switch(motor_state) {
			// If we are in the acceleration state, do run() and check our speed
			case MOTOR_ACCELERATING:
				myStepper.run();
				// If we have accelerated to the desired speed, switch
				// to the constant speed mode
				if(myStepper.speed() == DESIRE_SPEED) {
					motor_state = MOTOR_CONST_SPEED;
				}
				break;
			
			// If we are in the constant speed state, just do runSpeed()
			case MOTOR_CONST_SPEED:
				myStepper.runSpeed();
				break;
			
			default:
				break;
		}
	}

	if(command_recieved_to_dispense_pills) {
		//flag that we need to start dispensing pills
		flag_dispensing = 1;
		// Start the motor moving with acceleration 
		// (the distance must be large enough to reach the desired speed)
		myStepper.move(1000);
	}

	if(pills_dispensed > 24) {
		// When the 25th pill is counted, decrease the speed to 
		// to a safe dead-stop speed
		myStepper.setMaxSpeed(DEAD_STOP_SPEED);
		myStepper.setSpeed(DEAD_STOP_SPEED);
	}

	if(pills_dispensed > 29) {
		// When the 30th pill is counted, Set the position the motor is at 
		// right now to 0 to make the stepper think it has arrived 
		// and instantly go to zero velocity
		myStepper.setCurrentPosition(0);

		// Then flag that we are no longer dispensing
		flag_dispensing = 0;
	}
}
```

And that's the fancy shmancy version. In plain English, when it is time to dispense pills, we have the motor start moving with acceleration to some point ahead of it and set the sate of the finite state machine to `MOTOR_ACCELERATING` . As it accelerates, we keep calling `run()` and keep checking if the motor speed has reached the desired speed. Once the speeds match, we switch over to just running constant speed. Then finally, just as before, when the 25th pill is counted, we lower the speed we want, and then once the 30th pill is counted we do the dead stop. Tada!

**Pro Tip:** Having this happening for multiple motors in a single function is gonna get real messy real fast. I suggest taking the functionality and moving it into separate functions to make it easier to deal with like so:

```C
#define MOTOR_ACCELERATING 1
#define MOTOR_CONST_SPEED 2
#define DESIRE_SPEED 200 // Whatever the target speed is

int motor_state = MOTOR_ACCELERATING;

int flag_dispensing = 0;

void pillFillSetup() {
	// The acceleration to be used when coming up to speed 
	myStepper.setAcceleration(ACCELERATION);
	
	myStepper.setMaxSpeed(SPEED);
}

void pillFillUpdate() {
	// If we are dispensing, do the stuff to make the motor move
	if(flag_dispensing == 1){
		// Switch case to implement the finite state machine
		switch(motor_state) {
			// If we are in the acceleration state, do run() and check our speed
			case MOTOR_ACCELERATING:
				myStepper.run();
				// If we have accelerated to the desired speed, switch
				// to the constant speed mode
				if(myStepper.speed() == DESIRE_SPEED) {
					motor_state = MOTOR_CONST_SPEED;
				}
				break;
			
			// If we are in the constant speed state, just do runSpeed()
			case MOTOR_CONST_SPEED:
				myStepper.runSpeed();
				break;
			
			default:
				break;
		}
	}

	if(command_recieved_to_dispense_pills) {
		//flag that we need to start dispensing pills
		flag_dispensing = 1;
		// Start the motor moving with acceleration 
		// (the distance must be large enough to reach the desired speed)
		myStepper.move(1000);
	}

	if(pills_dispensed > 24) {
		// When the 25th pill is counted, decrease the speed to 
		// to a safe dead-stop speed
		myStepper.setMaxSpeed(DEAD_STOP_SPEED);
		myStepper.setSpeed(DEAD_STOP_SPEED);
	}

	if(pills_dispensed > 29) {
		// When the 30th pill is counted, Set the position the motor is at 
		// right now to 0 to make the stepper think it has arrived 
		// and instantly go to zero velocity
		myStepper.setCurrentPosition(0);

		// Then flag that we are no longer dispensing
		flag_dispensing = 0;
	}
}

void setup() {
	pillFillSetup();
}

void loop() {
	pillFillUpdate();
}
```