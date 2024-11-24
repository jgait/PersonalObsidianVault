# Problem 1
![[Pasted image 20230918222530.png]]

Substituting $v(t) = r(t) - \pmb{K}x(t)$ into the state space equations yields,

$$ \dot{x}(t) = (A - BK)x(t) + Br(t) \tag 1$$
$$ y(t) = (C-KD)x(t) + Dr(t) \tag 2$$ Taking the Laplace of these results in 3 and 4 from which new A, B, C and D matrices can be gotten
$$ sX(s) = (A - BK)X(s) + BR(s) \tag 3$$
$$ Y(s) = (C-KD)X(s) + DR(s) \tag 4$$
Finally, plugging into the formula for state space to transfer function, 
$$\boxed{\frac{Y(s)}{R(s)} = (sI - (A-BK))^{-1} B(C-KD)+D}$$
And the characteristic equation,
$$ \boxed{ \text{Ch.Eq.} = det(sI-A+BK) = 0 }$$

<div style="page-break-after: always;"></div>

# Problem 2
![[Pasted image 20230918225151.png]]

For the desired poles, the characteristic equation must be $s^2+6s+18 = 0$. Applying the gain matrix K to the existing state space gives the characteristic equation,
$$s^2 + k_1s + k_2 = 0$$
Thus, to satisfy the desired cheq. with this formula, the gain matrix must be, 
$$\boxed{\pmb{K} = \begin{bmatrix} 6 & 18 \end{bmatrix}}$$
Please see MATLAB program and end of document for code used in this problem.

<div style="page-break-after: always;"></div>

# Problem 3
![[Pasted image 20230918230256.png]]

For the desired poles, the characteristic equation must be $s^2+3s+9 = 0$, since the general cheq is $s^2 + 2\zeta\omega_ns + \omega^2_n = 0$. Applying the gain matrix K to the existing state space gives the characteristic equation,
$$s^2 + 4s + k_1s + k_2 = 0$$
Thus, to satisfy the desired cheq. with this formula, the gain matrix must be, 
$$\boxed{\pmb{K} = \begin{bmatrix} -1 & 9 \end{bmatrix}}$$
Please see MATLAB program and end of document for code used in this problem.

<div style="page-break-after: always;"></div>

# Problem 4
![[Pasted image 20230918230650.png]]
*4a)*
$$\boxed {\pmb{X} = \begin{bmatrix} \theta \\ \dot \theta \end{bmatrix} \qquad 
\pmb{\dot X} = 
\begin{bmatrix} 0 & 1 \\
			  -4 & 0 \end{bmatrix} \pmb{X}
+ \begin{bmatrix} 0 \\ 1 \end{bmatrix} T_c}$$
*4b)*
$$\boxed{det\left (\begin{bmatrix} s & 0 \\ 0 & s \end{bmatrix} - \begin{bmatrix} 0 & 1 \\ -4 & 0 \end{bmatrix}\right)}$$
*4c)*
Desired poles are $s=-2\pm2$. First obtain cheq. with K matrix included
$$det\left (\begin{bmatrix} s & 0 \\ 0 & s \end{bmatrix} - \begin{bmatrix} 0 & 1 \\ -4 & 0 \end{bmatrix} + \begin{bmatrix} 0 \\ 1 \end{bmatrix} \begin{bmatrix} k_1 & k_2 \end{bmatrix} \right)$$
$$det\left (\begin{bmatrix} s & -1 \\ 4+k_1 & s+k_2 \end{bmatrix}\right) = s^2 + k_2s + 4 + k1 = 0$$
then, find desired cheq.
$$(s-(2+2j))(s-(-2-2j)) = s^2+4s+8 = 0$$
Thus gain matrix K is,
$$\pmb{K} = \begin{bmatrix} 4 & 4 \end{bmatrix} $$

<div style="page-break-after: always;"></div>

# Problem 5
![[Pasted image 20230918232509.png]]
![[Pasted image 20230918232529.png]]
*a)* 
First, compute poles to check stability. $\frac{3}{s^2+2s-3}$ has poles at -3 and 1. The positive pole makes it unstable meaning it will have no final value as it will blow up and go to infinity or negative infinity

*b)*
Checking poles shows poles @ $-1 \pm \sqrt{2}i$ meaning the system is stable. Thus the final value for a unit step can be computed as show below
$$ \boxed{\lim\limits_{s \to 0} = s \frac{3}{s^2+2s+3} \frac{1}{s} = \frac 3 3 = 1}$$








