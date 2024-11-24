#VST2 #VariableStiffnessTreadmill #HORCLab #Mechatronics #Outline

### A. Initial Specifications (Design Motivation)

##### Improvements over VST1
1. Bi-Compliant (vs mono-compliant on VST1)  
	1. Controllable on both sides independently
2. Linear Displacement (vs angular on VST1)  
	1. Justification - Minimum gate modification
3. Useful stiffness range (**Specific numbers?** Ultra-low stiffness on VST1 doesn't seem to be used)
	1. Justification - This is where we want to do the research based on previous work (Doesn't overly affect gait) Stiffness range is 1MN/m to 10 kN/m
4. Resolution of stiffness - Use the equation and the min change we can make, define a range
	1. Justification - Being able to simulate various surfaces accurately
5. Treadmill velocity range from (-4.47 m/s to 4.47 m/s) 
	1. Justification - Range to accommodate walking and running
6. **NOT in table** Stiffness that is independent of foot placement (without closed-loop control)
	1. Justification - 
7. Lower step-up height for patient comfort and safety  
8. Wider treadmill/belts for patient comfort and safety
9. longer belts for patient comfort and safety  
10. minimum inter-belt distance  
	1. Safety, comfort, minimum inter-belt distance -> safety, comfort, minimum gate modification
	2. Go deep, describe how the treadmill length was decided on, target length was determined 
	3. Compare to vertec
11. force sensing
	1. Installed force sensors on both belts in order to sense GRFs
13. ability to do independent speed control between the belts
	1. Independent speed control of the belts is an existing method for pertubring the gait

Add a table for the final specs

##### Other driving Factors
1. Maximum stiffness set time - 0.8 seconds 
2. Minimal platform inertia
3. Minimal Linear movement friction

##### Initial Specifications Section Outline
1. {I} Brief intro sentence or two about design how design started by outlining new specs for VST 2.0 with the goal of improving on weak points in VST 1.0
2. Run through every spec that is an improvement over VST 1.0
	1. {A} Paragraph about the variable stiffness changes
		1. Bi-Compliant (vs mono-compliant on VST1, adds the ability to perturbe each leg or both legs)  
		2. Linear Displacement (vs angular displacement on VST1, improves walking behavior as there is no pitching)  
		3. Useful stiffness range 1MN/m to 10kN/m (most experiments happen at aroudn 20 kN/m, Ultra-low stiffness on VST1 was not used)
		4. Stiffness that is independent of foot placement (without closed-loop control)
		5. MAYBE force sensing on both belts (for a full understanding of gait behavior)
	2. Paragraph about treadmill changes
		1. Treadmill velocity range comparable to VST1 (Majority of experiments are done at a walking speed around 1 m/s) 
		2. Ability to do independent speed control between the belts (this has been used previously to modify gait behavior and is something we may want to experiment)
		3. minimum inter-belt distance (patient comfort and safety as well as minimal impact on gait)
		4. MAYBE force sensing on both belts (for a full understanding of gait behavior)  
	3. Sentence or two about improving safety and comfort
		1. longer belts (patient comfort and safety )
		2. Lower step-up height (patient comfort and safety)
		3. Wider treadmill/belts (patient comfort and safety)
		4. **PROTO** Increasing subject comfort and safety was also an important factor and resulted in specification of a step-up height of less than 24 inches, as well as longer and wider treadmill belts than VST 1.0.
3. Paragraph about additional driving factors
	1. Minimal stiffness set time (within one swing phase)
	2. Minimal platform inertia (for good response)
	3. Minimal linear motion friction (for good response)
	4. **PROTO** In order to ensure that large stiffness changes could be made within the time available between steps, it was specified that the maximum stiffness set time be less than that of an average person's swing phase when walking. Additionally, to ensure a good system response, goals were set to minimize both platform inertia and linear travel friction.

### B. System Design 

##### Intro to the variable stiffness mechanism and the idea that it is the part that changes the stiffness
1. The primary function of the VST is to provide a variable stiffness surface that is accurate, repeatable, and predictable.   

##### Explanation of VSM operating principle
1. Intro one-liner about how the VSM works
	1. **PROTO Sentence** At it's core, the variable stiffness mechanism (VSM) is a class-three-lever based system that modifies the Mechanical Advantage between the platform and a set of fixed stiffness primary springs to create an effective stiffness at the platform which is dependent upon the geometric configuration of the system.
2. Explain components and how they interact (Platform, VSM Carriage, Linear Rails, Lead Screw, Main VSM Lever Arm, Webbing, Primary Springs, Linear Rails)
	1. **PROTO Paragraph:** The treadmill platform (COLOR) which is constrained to move vertically by the vertical linear guides (COLOR) is supported by the carriage (COLOR) via the platform linear rails (COLOR). The carriage serves as the moveable connection point between the platform and the VSM assembly and rides on the VSM linear rails (COLOR). Together, the carriage, VSM linear rails, and the Leadscrew-Motor assembly (COLOR) form a linear actuator that allows the carriage to be positioned anywhere along the VSM lever arm (COLOR). The VSM lever arm, pivots about the axle (COLOR) towards the rear of the treadmill. Towards the front of the treadmill, the end of the VSM lever arm is supported by the spring webbing (COLOR) which is fixed to the frame and wraps down and around a roller before coming parallel to the VSM lever arm. Under the VSM lever arm, the other end of the spring webbing connects to the primary springs (COLOR) which are fixed to the bottom of the VSM lever arm. Thanks to the roller, any deflection of the VSM lever arm requires extension of the spring webbing and a corresponding extension in the primary springs, generating a spring force that acts at the end of the VSM lever arm and resists the force applied to the platform. 
	2. **IMAGE** Color in the cad and show a screenshot (maybe multiple to have enough colors)
3. Explain the relationship between system configuration and effective stiffness
	4. **PROTO Paragraph:** This VSM design creates a system that varies the mechanical advantage between the platform and the main springs via changes in the carriage position, and by extension the point of platform force application to the VSM lever arm. Maximum stiffness is achieved when the carriage is placed over the pivot point of the VSM lever arm. With the carriage in this position, a force applied to the platform is transferred directly down through the carriage, through the VSM axle and into the frame of the VST, resulting in no platform deflection. As the carriage is moved away from the pivot and towards the end of the lever arm, the Mechanical Advantage between the platform and the primary springs increases from 0 and approaches 1, resulting in a lower effective stiffness for carriage positions further from the pivot. For any carriage position in this range, a force applied to the platform results in a linear deflection of the platform, creating an angular deflection in the VSM and forcing the primary springs to elongate via the spring webbing. This generates a spring force that resists the force applied to the platform and results in an effective platform stiffness which is proportional to the current carriage position and the spring coefficient of the primary springs.
4. Derivation of stiffness equation
	1. **PROTO Intro:** From this VSM design, a kinematic analysis was performed to derive the relationship between system parameters and the effective spring force. 
	2. Include Panos derivation that he is working on

##### Discussion of component and design selection
1. Error of ignoring angle was evaluated -> Resulted in a longer lever arm being selected to minimize small angle approximation error
2. Target stiffness range -> informed selection of springs

**PROTO Paragraph:**  With a general system design finalized and governing equations derived, the design space was explored in order to select specific components and dimensions for the system. As seen in equation **_Eqn_Num_**, the true effective stiffness at the platform is a function of primary spring stiffness, position of the carriage, and the angle of deflection of the VSM lever arm. However, to simplify control, it is desirable to avoid having to measure the angle of the VSM and instead assume that the angles in the VSM are always near enough to zero to be ignored. To understand the ramifications of this assumption, the error between the true effective stiffness (**_Eqn_Num_**) and the assumed effective stiffness (**_Eqn_Num_**) was calculated for a variety of different spring stiffnesses, lever arm lengths, and carriage positions. This analysis revealed that a longer lever arm minimizes the error arising from small angle assumption and resulted in the selection of a 1 meter lever arm, which corresponds to a maximum stiffness error of **_INSERT PERCENTAGE_**. With the length of the lever arm set, equation **_Eqn_Num_** was used to determine the primary spring stiffness needed to achieve the specified variable stiffness range of 1 MN/m to 10-15 kN/m. Based on this calculation, a bank of two 4.3 kN/m springs in parallel were selected as the primary spring, yielding a primary spring stiffness of 8.6 kN/m and a minimum effective stiffness of 12 kN/m. The rest of the VSM was then designed to accommodate the 1 meter lever arm and the two 4.3 kN/m springs in parallel. 

### C. Control Structure

-- provide big control block diagram including all components/processes, signals and rates of communication for each one of then. talk about all sensors and actuators in the text, and the overall architecture controlling effective stiffness and speed for each belt, and measuring vertical displacement and force. Make sure the diagram has inputs Kd and speed for each belt. Include GUI