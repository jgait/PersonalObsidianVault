#HeatTransfer 
### Finite Difference formulas

**Assumptions:** 
1. $\Delta x = \Delta y = \Delta \ell$
2. Starting temp dist is known
3. $Fo = \frac{\alpha \Delta t}{\Delta \ell^2}$
4. $Bi = \frac{h\Delta \ell}{k}$
5. For 2d sim stability $Fo \le 1/4$

**Interior Node:** 
For a 2d interior node, the new node temperature is given by (from book):

$T_{x,y}^{n+1} = Fo(T_{x+1,y}^{n} + T_{x-1,y}^{n} + T_{x,y+1}^{n} + T_{x,y-1}^{n})+(1-4Fo)T_{x,y}^{n}$

$\frac{dT}{dX}|_{x,y}$


This can be conceptualized like the below image, the new blue temp is a function of the previous temp and the surrounding node temperatures.
![[Finite_Diff_Concept_Model.png]]

*In python*
```python
def interior_node(T_cur, T_top, T_bot, T_left, T_right):
    global Fo
    return Fo * (T_right + T_left + T_bot + T_top) + (1 - 4 * Fo) * T_cur
```

**Constant Heat Flux Boundary Node:**
![[Node for constant heat flux.png|300]]
Starting with an energy balance of the above figure
$q_{cond\_top} + q_{cond\_bottom} + q_{cond\_left} + q_{const\_q} = \rho{C_p}A \frac{dT}{dt}$

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

**[[Convection]] Boundary Node:**

![[ConvectionBoundaryNodeImg.png|300]]

Starting with an energy balance of the above figure
$q_{cond\_top} + q_{cond\_bottom} + q_{cond\_left} + q_{conv} = \rho{C_p}A \frac{dT}{dt}$

$(k \frac{\Delta x}{2} \frac{T_{x,y-1}^{n} - T_{x,y}^{n}}{\Delta y})+(k \frac{\Delta x}{2} \frac{T_{x,y+1}^{n} - T_{x,y}^{n}}{\Delta y})+(k \Delta y \frac{T_{x-1,y}^{n} - T_{x,y}^{n}}{\Delta x}) + hd\Delta y (T_\infty - T_{x,y}^{n}) = \rho{C_p}A \frac{dT}{dt}$

Applying $\Delta x = \Delta y  = \Delta \ell$ and canceling

$\Delta \ell^2 \frac{\rho C_p}{k} \frac{T_{x,y}^{n+1} - T_{x,y}^n}{\Delta t} = \frac{1}{2}(T_{x,y-1}^{n} - T_{x,y}^n) +  \frac{1}{2}(T_{x,y+1}^{n} - T_{x,y}^n) + (T_{x-1,y}^{n} - T_{x,y}^n) + \frac{h \Delta \ell}{k} (T_\infty - T_{x,y}^n)$

simplifying and converting to common non-dimensional numbers like $Bi$ and $Fo$

$2 \frac{1}{Fo} (T_{x,y}^{n+1} - T_{x,y}^n) = T_{x,y-1}^n + T_{x,y+1}^n + 2T_{x-1,y}^n + 2(Bi)T_\infty - 4T_{x,y}^n + 2(Bi)T_{x,y}^n$ 

Finally, isolating $T_{x,y}^{n+1}$ on the left side

$T_{x,y}^{n+1} = \frac{Fo}{2} (T_{x,y-1}^n + T_{x,y+1}^n + 2T_{x-1,y}^n + 2(Bi)T_\infty + (-4+2Bi)T_{x,y}^n)+T_{x,y}^n$

*Generalized In python:*
```python
def conv_boundary_node(T_cur, T_opposite, T_adj_node_1, T_adj_node_2, delta_l):
    global Fo
    global Bi
    global h
    global T_inf
    return (Fo/2) * (T_adj_node_1 + T_adj_node_2 + 2*T_opposite + 2*Bi*T_inf + (-4+2*Bi)*T_cur) + T_cur  
```

Where:
	`T_cur` is the temp of the target node to be evaluated
	`T_opposite` is the temp of the adjacent node which is opposite the side of our target node that has the constant heat flux on it
	`T_adj_1` and `T_adj_2` are the other two temperatures that are adjacent to the target node and catty corner to the heat flux side of the target node
	`delta_l` is the distance between nodes ($\Delta \ell$)

**Constant Temp Boundary Node**
For a node exposed to at least one constant temp condition, the temperature will always be $T_{const}$. As an equation this looks like

$T_{x,y}^{n+1} = T_{const}$

*Generalized In python:*
```python
def const_temp_boundary_node(T_cur, T_opposite, T_adj_node_1, T_adj_node_2, delta_l):
    global T_const
    return T_const
```

**Note:** there is no need to treat corners separately, as every corner has a constant temperature boundary and therefore does not deviate from the constant temperature.

## Part 6 - Energy Balance

$q_\text{flux} + q_\text{const1} + q_\text{const2} + q_\text{conv} = 0$

Applying Fourier equation

$q''\ell - (-k\Delta T_\text{top}) - (-k\Delta T_\text{bottom}) - h\ell\Delta T = 0$ 

Discretizing (Where $N$ is the number of nodes on a side minus 1)

$q''\ell - (k \Delta\ell \sum_{x=0}^N(\frac{T_{const\_top} - T_{x, y_{top}+1}}{\Delta \ell})) - (k \Delta\ell \sum_{x=0}^N(\frac{T_{const\_bot} - T_{x, y_{bot}-1}}{\Delta \ell})) - h( \sum_{y=0}^N\Delta \ell(T_{x_{right}, y} - T_\infty)) = 0$

$q''\ell - (k\sum_{x=0}^N(T_{const\_top} - T_{x, y_{top}+1})) - (k \sum_{x=0}^N(T_{const\_bot} - T_{x, y_{bot}-1})) - h( \sum_{y=0}^N \Delta\ell (T_{x_{right}, y} - T_\infty)) = 0$

At the end, fix the signs based on the temperature gradient



 