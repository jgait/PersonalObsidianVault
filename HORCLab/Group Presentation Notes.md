## Shubham - 2024-02-16
- We be making digit work with a human using a new control framework
- Low Level --> model predictive control (MPC)
- Basic walking frameworks are insufficient for collaborative lifting and movement
- An admittance controller relative to the box can be used to provide compliant behavior to the MPC 
- With this model the state is fixed after each step because system inputs only come during steps (the step is the thing that affects the system)
- 