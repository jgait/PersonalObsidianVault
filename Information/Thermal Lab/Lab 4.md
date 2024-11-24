## Numerical finite diff solver

We start with a 2d array where every node has a temperature. To compute the new temperature of that node at the next time step, plug it into the explicit finite difference formula that corresponds to the node's edge conditions (in this case, interior node, const flux boundary node, const temp boundary node, or convection boundary node). Then do this over and over again to evaluate the solution over time. It seems like it is most useful to build a 3d array where x and y are the nodes on the plate and z is time.

### Finite Difference formulas

**Assumptions:** 
1. $\Delta x = \Delta y = \Delta \ell$
2. Starting temp dist is known
3. $Fo = \frac{\alpha \Delta t}{\Delta \ell^2}$
4. For 2d sim stability $Fo \le 1/4$

**Interior Node:** For a 2d interior node, the new node temperature is given by (from book):
$T_{x,y}^{n+1} = Fo(T_{x+1,y}^{n} + T_{x-1,y}^{n} + T_{x,y+1}^{n} + T_{x,y-1}^{n})+(1-4Fo)T_{x,y}^{n}$
This can be conceptualized like the below image, the new blue temp is a function of the previous temp and the surrounding node temperatures.
![[Finite_Diff_Concept_Model.png]]

*In python*
```python
def interior_node(T_cur, T_top, T_bot, T_left, T_right):
    global Fo
    return Fo * (T_right + T_left + T_bot + T_top) + (1 - 4 * Fo) * T_cur
```

**Constant Heat Flux Boundary:**
![[Node for constant heat flux.png|300]]
Starting with an energy balance of the above figure
$q_{cond\_top} + q_{cond\_bottom} + q_{cond\_left} + q_{const} = \rho{C_p}A \frac{dT}{dt}$

$(k \frac{\Delta x}{2} \frac{T_{x,y-1}^{n} - T_{x,y}^{n}}{\Delta y})+(k \frac{\Delta x}{2} \frac{T_{x,y+1}^{n} - T_{x,y}^{n}}{\Delta y})+(k \Delta y \frac{T_{x-1,y}^{n} - T_{x,y}^{n}}{\Delta x}) + \Delta y q'' = \rho{C_p}A \frac{dT}{dt}$

Hefty algebraic manipulation to obtain
$T_{x,y}^{n+1}=\frac{Fo}{2}(T_{x,y-1}^{n} + T_{x,y+1}^{n} + 2T_{x-1,y}^{n} + \frac{2 \Delta \ell q''}{k} - 4 T_{x,y}^{n})+T_{x,y}^{n}$

*Note:* This was derived from a basic energy balance equation for a node with constant heat flux on the right and conduction on the other 3 sides

*Generalized In python:*
```python
def const_flux_boundary_node(T_cur, q, T_opposite, T_adj_node_1, T_adj_node_2, delta_l, conduction_coeff):
    global Fo
    return (Fo/2) * (T_adj_node_1 + T_adj_node_2 + 2*T_opposite + ((2*delta_l*q)/conduction_coeff) - 4*T_cur) + T_cur  
```

Where:
	`T_cur` is the temp of the target node to be evaluated
	`q` is the constant heat flux at the boundary
	`T_opposite` is the temp of the adjacent node which is opposite the side of our target node that has the constant heat flux on it
	`T_adj_1` and `T_adj_2` are the other two temperatures that are adjacent to the target node and catty corner to the heat flux side of the target node
	`delta_l` is the distance between nodes ($\Delta \ell$)
	`conduction_coeff` is the coefficient of conduction for the plate material ($k$) 

**Constant Temp Boundary Node:** To create a constant temp boundary condition, set the temp of the nodes with this boundary condition to the same temperature every time we evaluate it

*In python:*
```python
def const_temp_boundary_node(T_cur, const_temp):
    return const_temp
```

### Node Evaluation Logic

**Representing the plate from lab as an array of size 4x4, the plate looks like thi**s:
```
Format for indexes is Tyx to match with the way arrays index

				<Const Temp>
             [T00][T01][T02][T03]
<Const Flux> [T10][T11][T12][T13] <Const Temp>
			 [T20][T21][T22][T23] 
			 [T30][T31][T32][T33]
			     <Const Flux>
```

**The logic for evaluating the nodes goes as follows:**
```
* is a wild card -> eg. T*0 includes T10, T20, etc..

if node is on top edge (is T0*):
	evaluate as const temp

elif node is on right edge (is T*3):
	evaluate as const temp

elif node is on left edge (is T*0):
	evaluate as const flux

elif node is on bottom edge (is T3*):
	evaluate as const flux

else:
	evaluate as interior
```
*Note:* it is important to have the const temp conditions come first because we want the const temp to override the temp due to const flux. Having const temp come first does this by ensuring that the top left corner and bottom right corners, which have const temp and const flux borders, are caught by the const temp condition first.