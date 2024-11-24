## Topics to discuss
1. Now that the Odrives seem to be working well, what is the next step?
2. Other hardware changes still need to happen (Do we want to complete all of them before moving to star)
3. We need to find the "Sweet Spot" update frequency for the forcemats that will eliminate the weird spikes in acquisition time.

**The argument for keeping the Odrives:** The Odrives+motors work well when we aren't battling issues and provide a lot of flexibility for us to modify and configure them. The VST2 mechanical embodiment is optimized to fit the specific motors we have now, and we have started building the software around the Odrives.

**The argument for ditching the Odrives:** I still have gripes with the long term reliability of the motors+Odrives. Even with the Odrives configured to limit the current to the VSM motors to the 65A quoted on the website, I question the long term performance of the motors, just because we do not have any guarantees on what testing was done and what lifetime can be expected. In the same vein, we have no info on the mechanical construction of the motors (eg. what is the rated lifetime of the bearings?) Finally, something like the ground loop issue cropped up out of nowhere after months of operation. If something else weird like this happens, having new motors with a support department we can get on the phone might prove very helpful

**The path forward I'd push for:** The reliability of the Odrives is not the only factor in the decision to motor swap. The plan also relies on finding new motors that meet our specs, as well as the viability of adapting the new motors to the VST. I vote that we work to define the motor spec (Via Brad's tests), then search for motors, and finally see if we could figure out how to integrate the new motors into the system. In short, if it is doable, I am still for leaving the Odrives behind. Additionally, there is time for the motor swap as there are still other hardware changes/fixes outstanding, some of which need to be addressed before experiments can start.

#### Other Hardware Changes Still on the Docket
A. Figure out how to remedy belt motor crashing into CWS bearing at 80mm deflection (Major Issue, we risk destroying the belt motors if this isn't fixed)
B. Install Force Mats (Needed prior to experiments)
D. Simplified CWS to Platform connection for faster platform removal
E. Easily removable bumpers for faster platform removal
F. Adjust VSM carriage stack height (To eliminate ~5mm bumper compression at inf stiffness)
