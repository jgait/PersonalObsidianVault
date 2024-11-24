# Binary Clock Info
#RaspberryPi #CProgram
A quick rundown of how to do various things that will be useful for the binary clock project

## INDEX
1. Getting system time
2. Writing to the LED display
3. Cleaning up and exiting on ctrl-c keyboard command
4. Compiling the program

## Getting the System Time
C provides a library that can be used to get the system time of the computer. To use it, first `#include` the library (It should be pre-installed on the pi)
```C
#include <time.h>
```
Then grab the current time like so
```C
char hours;
char minutes;
char seconds;

// Make the silly time object
time_t t = time(NULL);
struct tm *current_time = localtime(&t);

// Parse the times
hours = current_time->tm_hour;
minutes = current_time->tm_min;
seconds = current_time->tm_sec;
```

## Writing to the Display
Writing to the display revolves around setting the colors of individual pixels in a `frame_buffer` object that sets up all the pixels in a grid. To start, include the `framebuffer.h` library
```C
#include "framebuffer.h"
```
Then create a framebuffer object and set pixels as shown below
```C
pi_framebuffer_t* my_frame_buffer;
my_frame_buffer = getFBDevice();

// Clear the display to ensure that there are no leftover pixels
clearBitmap(my_frame_buffer->bitmap, getColor(0,0,0));

// Set the pixel in row 2, column 4 to blue
my_frame_buffer->bitmap->pixel[2][4] = getColor(0, 0, 255)
```
**A Note about pixel positions:**
One thing that's kinda goofy is that the LED array addresses pixels as `pixel[row][column]`. Thinking about this in xy coordinates, the syntax is `pixel[y_position][x_position]` so the pixel located at coordinate (x = 4, y = 3) would be addressed as `pixel[3][4]`. This mega messed me up at first. Also, another silly thing, the origin of the display is in the top left corner, not the bottom left corner. So the bottom row is row 7 and the top row is row 0. See the diagram below for the layout:

```
NOTE: pixel notation is pixel[y][x]

        	 x-coords
            0  1  2  3  4  5  6  7 
	 0 [ ][ ][ ][ ][ ][ ][ ][ ]         
	 1 [ ][ ][ ][ ][ ][ ][ ][ ]         
	 2 [ ][ ][ ][ ][ ][ ][ ][ ]        === For Orientation ===
y-coords 3 [ ][ ][ ][ ][ ][ ][ ][ ]         Raspberry Pi USB Port 
	 4 [ ][ ][ ][ ][ ][ ][ ][ ]             is over here
	 5 [ ][ ][ ][ ][ ][ ][ ][ ]        =======================
	 6 [ ][ ][ ][ ][ ][ ][ ][ ]
	 7 [ ][ ][ ][ ][ ][ ][ ][ ]
		 
```

## Cleaning Up and Exiting on Ctrl-C Keyboard Command
Ctrl-c will already exit the program on its own, however, to be memory safe we need to free the `framebuffer` object that we created at the start of the program. To do this, we can use the signal library to catch the ctrl-c interrupt that is sent to the program and act on it before exiting. First include the library

```C
#include <signal.h>
```

Then write a function that will be called when the ctrl-c keypress is received and free the framebuffer then exit the program

```C
// Signal handler to free the frame buffer before exiting
void ctrlCHandler(int signal_number){
	freeFrameBuffer(my_frame_buffer); // This needs to match the name of our framebuffer
	exit(signal_number);
}
```

Finally, link the `ctrlCHandler()` function to the ctrl-c keypress using signal at the start of your main function with the line below.

```C
signal(SIGINT, ctrlCHandler);
```

This links the `SIGINT` event (triggered by ctrl-c keypress) to our `ctrlCHandler()` function as a callback, causing it to run when the event is recieved.

## Compiling the program
The long way is that compilation is done with gcc like before. The one thing is that now we need to include the `framebuffer.o` file in the command so that it gets linked into the program. Make sure `main.c`, `framebuffer.h`, and `framebuffer.o` are all in the same folder, then run the following gcc command in the folder:

```
gcc main.c framebuffer.o -o my_output_filename
```

The nice thing is that Mr. Silber Man wants us to use a Makefile for this project to simplify building it. So create a new file named `makefile` (no .txt or anything, just the name) and put the following in it

```
all:
	gcc main.c framebuffer.o -o my_output_filename
```

Then to compile the program, just type `make` in the command line and the whole thing will build. Make sure to submit the makefile and the main.c file.