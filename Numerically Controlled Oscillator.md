A numerically controlled oscillator is a structure/pattern/algorithm that allows for digitally generating a sine wave of arbitrary frequency. It consists of a **phase accumulator** feeding into a **phase to amplitude** converter. If phase control is also desired, a phase control word can also be specified as shown below

![[Pasted image 20240402193326.png]]
**Links**
[Wikipedia on NCOs](https://en.wikipedia.org/wiki/Numerically_controlled_oscillator)
[A resource with nice diagrams and a C++ implementation](https://zipcpu.com/dsp/2017/12/09/nco.html)
### Theory
1. One cycle of a sine wave of frequency $f$ is sampled at sample frequency $f_s$ and stored in a lookup table.
2. To construct an arbitrary sine wave, a desired frequency $f_d$ is fed into a phase accumulator. The phase accumulator takes the previous phase $\phi(n-1)$ and adds on the phase step $\frac{f_d}{f_s}$ to get the current phase $\phi(n)$. The integer part of $\phi$ is dropped, resulting in a 
$$\phi(n) = \phi(n-1) + \frac{f_d}{f_s}$$
4. The phase is then fed to the phase summer that adds a desired phase control word that corresponds to a phase shift (truncated to account for wrapping)
5. Finally, the phase is fed to a Look Up Table that spits out the sine wave at the desired amplitude
### Binary Implementation

**LUT Creation for the Phase to Amplitude Converter (PAC)** 
One cycle of a sine wave is sampled into a Look Up Table with $2^M$ rows (Generally $M$ is chosen such that it works well with a default data type I.E.  for 1 byte, $M = 8$)

**Binary Phase Accumulator (PA)**
The phase accumulator takes a frequency control word (really the phase step in goofy units) and adds it to the current phase stored in a variable of width $N$ (generally 16 to 64). The fractional part of the phase is all that is stored here as this is what matters and what ranges from 0 to 1 as the waveform goes through one period. Overflow of the variable is used to ensure that the integer part of the phase is dropped. The output of the Phase Accumulator is the current phase.

Going from desired frequency $f_d$ to the frequency control word (FCW) depends on the clock frequency $f_{clock}$ and the size of the phase register $2^N$ and is given by the following equation,
$$ \text{FCW} = (2^N)\frac{f_d}{f_{clock}}$$
The phase accumulator is constructed such that its register width $N$ is significantly larger than the input width of the LUT $M$. 

**Phase Shift Summer**
To implement a constant phase offset, the accumulated phase coming out of the PA is summed with a phase control word (PCW). The equation for the PCW in terms of phase $\phi$ from $0$ to $2\pi$ is given by,
$$\text{PCW} = 2^N\frac{\phi}{2\pi}$$
This is added on, again allowing integer overflow to handle the roll over if the PCW exceeds a value equivalent to $2\pi$.

**Accumulated Phase Truncation**
Since the width of the phase variable ($N$) is so much larger than the width of the PAC input ($M$), the phase variable must be truncated. This is done by taking the top $M$ bits of $N$ and passing them to the phase to amplitude converter. This results in the remaining $N-M$ bits of the phase serving as a sort of secondary error accumulator that allows generating frequencies that are not possible with only the integer values of the LUT. This can be thought of as a gradual build up of error until the phase accumulator output switches/slips over to the proper PAC input.

![[Pasted image 20240402205318.png|600]]

**Phase to Amplitude Converter**
The phase to amplitude converter simply takes the truncated phase value and returns the sine amplitude at that row of the LUT. 

**Problems**
Because it is digitally constructed, there are spurious products