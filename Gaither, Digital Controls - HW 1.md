## Problem 1
![[Pasted image 20230912231348.png]]
**1a)** *What is the sampling period, in seconds, and the acquisition frequency, in Hz, of the laser scanner?*
$$ T_{sample} = \frac{\pu{60 sec}}{\pu{1200samples}} = \pu{0.05 sec} $$
$$ F_{sample} = \frac{1}{T_{sample}} = \pu{20 Hz} $$

**1b)** *What is the minimum resolution in (m) for the surroundings of the robot, given by the scanner?*

The scanner returns the distance to the object as a single byte. Thus, the range 0-15m must be mapped to the range of one byte, a.k.a. 0-255. Doing this gives,
$$ \delta_{min} = \frac{\pu{15 m}-\pu{0 m}}{256} = \pu{0.059 m}$$

**1c)** *If the robot controller needs information about the robot surroundings every 1 millisecond, and a minimum resolution of measurement of 1mm, what is the minimum desired bit rate for the serial communication?*

To achieve a resolution of measurement of 1mm, more bits will be needed for each reading. The minimum number of discrete steps is found by dividing the range by the desired resolution,
$$ {Steps}_{min} = \frac{\pu{15m}}{\pu{1mm}} = \pu{15000 Steps}$$
As the values sent are in binary, the fewest number of bits that can contain this number is 14, giving a maximum value of $2^{14} = 16384$. To determine the true resolution with this many bits, the range is divided by the number of steps,
$$ \delta_{actual} = \frac{\pu{15 m}}{\pu{16385 steps}} = \pu{0.00092m}$$
The minimum bit rate is found by dividing the number of bits to be sent per reading by the desired reading period as shown below, 
$$ Bitrate = \frac{\pu{14 bits}}{\pu{0.001 sec}} = \pu{14000 \frac{bits}{sec}}$$
Thus, by sending a $\pu{14 bit}$ measurement at a rate of $\pu{14 \frac{kBits}{sec}}$ a range measurement with a resolution of $\pu{0.92 mm}$ will be received every millisecond. 

<div style="page-break-after: always;"></div>

## Problem 2
![[Pasted image 20230913095108.png]]
![[Pasted image 20230913095001.png|center|400]]
Using the impedance method with position outputs $X_1$, $X_2$, and $X_3$, yields the system of equations shown below,

$$\begin{bmatrix} 4s^2+2s+6 & -2s & 0 \\ -2s & 4s^2+4s+6 & -6 \\ 0 & -6 & 4s^2+2s+6 & \end{bmatrix} \begin{bmatrix} X_1(s) \\ X_2(s) \\ X_3(s) \end{bmatrix} = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix} F(s)$$

Using MATLAB (please see end of document for code and output) to solve this system for the transfer function $\frac{X_3(s)}{F(s)}$ gives the final equation, 
$$\frac{X_3(s)}{F(s)} = \frac{3}{8s^4 + 12s^3 + 26s^2 + 18s}$$

<div style="page-break-after: always;"></div>

## Problem 3
![[Pasted image 20230913211550.png|700]]
![[Pasted image 20230913211510.png|center|400]]
Following a similar process to problem 3, an additional angle variable $\theta_1(t)$ was defined as being the rotational position associated with the $\pu{1 kg.m^2}$ mass on the left. Applying the impedance method yields the following system of equations for the system,

$$ \begin{bmatrix} s^2+2s+1 & -(s+1) \\ -(s+1) & 2*s+1\end{bmatrix} \begin{bmatrix} \theta_1(s) \\ \theta_2(s) \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix} T(s)$$

Solving for $\frac{\theta_2(s)}{T(s)}$ using MATLAB yields,
$$\frac{\theta_2(s)}{T(s)} = \frac{1}{2s(s+1)}$$

<div style="page-break-after: always;"></div>

## Problem 4
![[Pasted image 20230913220846.png]]
![[Pasted image 20230913220754.png|center|300]]
**4a)**
Using MATLAB's `tf2ss` functionality yields the following state space representation of transfer function (a). Please see the end of this document for MATLAB code and output.
$$ \pmb{X} = \begin{bmatrix} s^3X(s) \\ s^2X(s) \\ sX(s) \\ X(s) \end{bmatrix} 
\qquad s\pmb{X} = 
\begin{bmatrix} -5 & -1 & -5 & -13 \\ 
               1 & 0 & 0 & 0 \\ 
               0 & 1 & 0 & 0 \\ 
               0 & 0 & 0 & 0 \end{bmatrix}
\pmb{X} + \begin{bmatrix} 1 \\ 0 \\ 0 \\ 0 \end{bmatrix} R(s)$$
$$ \pmb{Y} = \begin{bmatrix} 0 & 0 & 8 & 10 \end{bmatrix} \pmb{X}$$


**4b)**
Again, using MATLAB, transfer function (b) can be represented in state space as shown below,
$$ \pmb{X} = 
\begin{bmatrix} s^4X(s) \\
				s^3X(s) \\ 
			    s^2X(s) \\ 
			    sX(s) \\ 
			    X(s) \end{bmatrix} 
\qquad s\pmb{X} = 
\begin{bmatrix} -9 & -13 & -8 & 0 & 0 \\ 
               1 & 0 & 0 & 0 & 0 \\ 
               0 & 1 & 0 & 0 & 0 \\ 
               0 & 0 & 1 & 0 & 0 \\
               0 & 0 & 0 & 1 & 0 \end{bmatrix}
\pmb{X} + \begin{bmatrix} 1 \\ 0 \\ 0 \\ 0 \\ 0 \end{bmatrix} R(s)$$
$$ \pmb{Y} = \begin{bmatrix} 1 & 2 & 12 & 7 & 6\end{bmatrix} \pmb{X}$$

<div style="page-break-after: always;"></div>

## Problem 5
![[Pasted image 20230913213808.png|500]]
To transform the transfer function into state-space, the intermediary function $X(s)$ is introduced, giving equation (1) shown below,
$$ T(s) = \frac{Y(s)}{R(S)} = \frac{Y(s)}{X(s)}\frac{X(s)}{R(s)} = \frac{(s^2+3s+8)}{(s+1)(s^2+5s+5)} \tag{1}$$
The transfer function is split into two separate equations giving equations (2) and (3) shown below,
$$ \frac{X(s)}{R(s)} = \frac{1}{(s+1)(s^2+5s+5)} \tag{2} $$
$$\frac{Y(s)}{X(s)} = (s^2+3s+8) \tag{3}$$
Equation (2) can then be rearranged to isolate the highest state derivative $s^3X(s)$, resulting in the following,
$$ s^3X(s) = R(s) - 6s^2X(s) - 10sX(s) -5X(s) \tag{4} $$
Equation (4) is then used to create the state-space A and B matrices, resulting in the state-space representation shown below where $\pmb{X}$ is the state variable,
$$ \pmb{X} = \begin{bmatrix} X(s) \\ sX(s) \\ s^2X(s) \end{bmatrix} \qquad s\pmb{X} = \begin{bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ -5 & -10 & -6 & \end{bmatrix} \pmb{X} +\begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} R(s) \tag{5}$$
The final piece of the state-space representation is the output $\pmb{Y}$ and is derived from equation (3), giving the equation,
$$ \pmb{Y} = \begin{bmatrix} 8 & 3 & 1 \end{bmatrix} \pmb{X}$$

