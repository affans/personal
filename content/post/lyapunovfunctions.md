---
title: "A Brief Exposition of Lyapunov Functions"
date: 2021-05-23
draft: true
---
https://www.ndsu.edu/pubweb/~novozhil/Teaching/480%20Data/13.pdf
https://stanford.edu/class/ee363/lectures/lyap.pdf

For a homogeneous dynamical system $$\mathbf{x}^{\prime} = f(\mathbf{x})\quad \text{  or  }\quad \frac{dx_i}{dt} = f_i(x_1, x_2, \ldots, x_n) \quad i = 1, 2, \ldots, n$$a point $\mathbf{x_e}$ is an equilibrium point if $\mathbf{f}(\mathbf{x_e}) = 0$. The system is said to be globally asymptotically stable if **every** trajectory $\mathbf{x}(t) \rightarrow \mathbf{x_e}$ as $t \rightarrow \infty$ or locally asymptotically stable near or at $\mathbf{x_e}$ if there exists $R > 0$ s.t. $\lVert \mathbf{x}(0) - \mathbf{x_e} \rVert \leq R \Rightarrow x(t) \rightarrow \mathbf{x_e}$ as $t \rightarrow \infty$. When $f$ is nonlinear, establishing stability is often difficult, but possible if the system is hyperbolic (or in other words all of the eigenvalues of the Jacobian have no real parts). This is called linearization, however has the drawback that it is essentially a local method. 

An alternative method is to use Lyapunov Functions (named after Russian mathematician Aleksandr Mikhailovich Lyapunov). Lyapunov functions are scaler functions defined on the phase space and may be used to prove the stability of an equilibrium of an ODE. It allows to make conclusions about the trajectories of a system without finding the trajectories (i.e. without solving the system). 

Before formally introducing Lyapunov functions, recall the notion of positive definite functions. In two variables, a function $V: U \subseteq \mathbb{R}^2 \rightarrow \mathbb{R}$ is said to be positive definite in $U$, if $V(x) > 0 \,\forall x \in U$ and $V(x) = 0$ if and only if $x = 0$. Positive definite functions have a strict isolated minimum at $x = 0$ and all sublevel sets of $g$ are bounded (equivalent to saying $g(x) \rightarrow \infty$ as $x \rightarrow \infty$). A general family of positive definite functions is given by 
$$ g(x) = ax_1^2 + 2bx_1x_2 + cx_2^2$$ where $a, b, c$ are parameters that must satisfy the conditions $a > 0$ and $ac - b^2 > 0$. The level sets of a postive definite function, i.e. the sets of the form $$(x \in \mathbb{R}^2 \mid V(x) = k)$$ are closed curves, atleast for some small $k$. See the figure below. 

Thinking geometrically, if the vector field of a dynamical system points inside any level set of a positive definite function, there is no way the orbits can leave the neighbourhood of the system. Algebraically, this can be expressed by finding the total derivative of $V$: 
$$\frac{dV}{dt} = \frac{\partial V}{\partial x_1}\frac{dx_1}{dt} + \frac{\partial V}{\partial x_2}\frac{dx_2}{dt}$$
This can be written as a scaler (dot) product of the ODE system and the gradient of $V$, i.e. 
$$\frac{dV}{dt} = \nabla V \cdot \mathbf{f}$$
From vector calculus, the gradient $\nabla V$ is always directed toward the greatest increase in $V$. Given that $V(x)$ is a positive definite function, it increases distance from the origin as $x \rightarrow \infty$. The velocity vector $f$, at any point, is tangent to the phase trajectory. 

#![lp function 1](/images/lyapunov-function1.svg)
#![lp function 2](/images/lyapunov-function2.svg)

Consider now, when the total derivative of $V$ is negative in some neighbourhood $R$ of the origin, i.e. $\frac{dV}{dt} < 0$. This implies that the angle $\theta$ between the gradient vector of $V$ and the velocity vector $f$ is greater than 90 degrees. The trajectory is falling "inwards" from some level set (see image). Clearly, if the derivative $\frac{dV}{dt}$ along a phase trajectory is negative, then the trajectory tends to the origin and the system is stable. When the derivative is positive, the trajectory moves away from the origin and is unstable. 

The strict formulation follows: 
Let $x_0 = 0$ be an equilibrium point of an ODE system (note non-zero equilibrium points can be translated to the origin). Let $V: U \subseteq \mathbb{R}^2 \rightarrow \mathbb{R}$ be a positive definite function on some neighbourhood of the origin. Then, 
1) if $\frac{dV}{dt} \leq 0 \, \forall x \in U, x \neq 0$ then $x_0$ is Lyapunov stable. 
2) if $\frac{dV}{dt} < 0 \, \forall x \in U, x \neq 0$ then $x_0$ is asymptotically stable. 
3) if $\frac{dV}{dt} > 0 \, \forall x \in U, x \neq 0$ then $x_0$ is unstable. 

Proofs can be found in any dynamical systems textbook or a Google search away. 

The third condition of the theorem is quite strong, since it is enough just one orbit to leave a neighborhood of the origin to call the equilibrium point unstable. However, the theorem is asking that all the orbits leave the neighbourhood for instability. Generalizing this fact is known as Chetaev's theorem. 

## Examples 
Examples of Lyapunov functions can be found in the accompanying blog post. 


<!-- ## Physical Interpretation of Lyapunov Functions 
Lyapunov functions have been used in various contexts (stability, convergence analysis, design of model reference adaptive systems, etc.). The Lyapunov approach is based on the physical idea that the energy of an isolated system decreases. A Lyapunov function maps scalar or vector variables to real numbers (ℜN → ℜ+) and decreases with time. The main attribute of the Lyapunov approach that makes it appealing for solving all the aforesaid engineering problems is that it is simple. The main obstacle to the use of Lyapunov theory is in finding a suitable Lyapunov function. -->