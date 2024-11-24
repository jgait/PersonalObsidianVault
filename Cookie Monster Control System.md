1. Get line angular position from line sensor algo
2. Calculate error from reference (0)
3. Run PD controller on error
4. Limit slew rate
### Line Sensor
Defines a bunch of different algorithms for calculating the angular position of the intersection between the line and the sensor bar.

**algoPureSum()**
> Returns a value resulting from the weight of each on element being added up. Relies on a weight matrix that can have non linear scaling

- *Good Stuff*
	- Helps to handle tight corners
- *Problems*
	- Need to tune weight as well as PID (maybe try setting the entire array to 1)

**algoLargestWeight()**
> Returns the largest weight of all the elements that are currently on. Relies on a weight array that can have non linear scaling
>