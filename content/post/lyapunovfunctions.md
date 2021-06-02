---
title: "A Brief Exposition of Lyapunov Functions"
date: 2021-05-23
draft: false
---
 <!-- https://www.ndsu.edu/pubweb/~novozhil/Teaching/480%20Data/13.pdf
https://stanford.edu/class/ee363/lectures/lyap.pdf
https://www.math24.net/method-lyapunov-functions 
https://www.math24.net/method-lyapunov-functions-page-2
https://inst.eecs.berkeley.edu/~ee222/sp16/LectureNotes/Lecture8.pdf
http://www.math.utk.edu/~freire/teaching/m431f14/Liapunov.pdf
https://www.egr.msu.edu/~khalil/NonlinearSystems/Sample/Lect_8.pdf

-->



For a homogeneous dynamical system $$\mathbf{x}^{\prime} = f(\mathbf{x})\quad \text{  or  }\quad \frac{dx_i}{dt} = f_i(x_1, x_2, \ldots, x_n) \quad i = 1, 2, \ldots, n$$a point $\mathbf{x_e}$ is an equilibrium point if $\mathbf{f}(\mathbf{x_e}) = 0$. The system is said to be globally asymptotically stable if **every** trajectory $\mathbf{x}(t) \rightarrow \mathbf{x_e}$ as $t \rightarrow \infty$ or locally asymptotically stable near or at $\mathbf{x_e}$ if there exists $R > 0$ s.t. $\lVert \mathbf{x}(0) - \mathbf{x_e} \rVert \leq R \Rightarrow x(t) \rightarrow \mathbf{x_e}$ as $t \rightarrow \infty$. When $f$ is nonlinear, establishing stability is often difficult, but possible if the system is hyperbolic (or in other words all of the eigenvalues of the Jacobian have no real parts). This is called linearization, however has the drawback that it is essentially a local method. 

An alternative method is to use Lyapunov Functions (named after Russian mathematician Aleksandr Mikhailovich Lyapunov). Lyapunov functions are scaler functions defined on the phase space and may be used to prove the stability of an equilibrium of an ODE. It allows to make conclusions about the trajectories of a system without finding the trajectories (i.e. without solving the system). 

Before formally introducing Lyapunov functions, recall the notion of positive definite functions. A function $V: U \subseteq \mathbb{R}^n \rightarrow \mathbb{R}$ is said to be positive definite in $U$, if $V(x) > 0 \,\forall x \in U$ and $V(x) = 0$ if and only if $x = 0$. Positive definite functions have a strict isolated minimum at $x = 0$ and are radially unbounded (i.e. as $x \rightarrow \infty$, $V(x) \rightarrow \infty$). This implies that all sublevel sets of $V$, that is the set $\\{x \mid V(x) \leq \alpha \\}$ for some $\alpha$ are closed curves (i.e. bounded). In $\mathbb{R}^2$ and $\mathbb{R}^3$, these sublevel sets are ellipses and ellipsoids, respectively. A general family of positive definite functions in $\mathbb{R}^2$ is given by $$ g(x) = ax_1^2 + 2bx_1x_2 + cx_2^2$$ where $a, b, c$ are parameters that must satisfy the conditions $a > 0$ and $ac - b^2 > 0$. 

Thinking geometrically, if the vector field of a dynamical system points inside any level set of a positive definite function at each point, then trajectories starting inside some neighbourhood within this level set can not escape. Algebraically, this can be expressed by finding the total derivative of $V$ at any trajectory. That is, if $\mathbf{x}$ is a solution to the autonomous system, then   
$$\frac{dV}{dt} = \frac{\partial V}{\partial x_1}\frac{dx_1}{dt} + \frac{\partial V}{\partial x_2}\frac{dx_2}{dt}$$
This can be written as a scaler (dot) product of the ODE system and the gradient of $V$, i.e. 
$$\frac{dV}{dt} = \nabla V \cdot \mathbf{f}$$
From vector calculus, the gradient $\nabla V$ is always directed toward the greatest increase in $V$. Given that $V(x)$ is a positive definite function, it increases distance from the origin as $x \rightarrow \infty$ and so one can imagine the gradient to be outward normal vectors. The velocity vector $f$, at any point, is tangent to the phase trajectory. Therefore, one can consider the angle the $\sigma$ between the two directions (see figures).   

#![lp function 1](/images/lyapunov-function1.svg)
#![lp function 2](/images/lyapunov-function2.svg)

Consider now, when the $\frac{dV}{dt} < 0$ in some neighbourhood of the origin. This implies that $V$ is decreasing along the solution and that the trajectory is falling "inwards" from some level set. Another geometric point of view is that the angle $\sigma$ between the gradient vector of $V$ and the velocity vector $f$ is greater than 90 degrees. So, if one could construct a Lyapunov function for an autonomous system, evaluate its derivative on a trajectory, find that the derivative is negative, the one may conclude the trajectory tends to the origin and the system is stable. Alternatively, when the derivative is positive, the trajectory moves away from the origin and the system is unstable. 

Lets provide a strict formulation along with a proof. 
Let $x^\ast = 0$ be an equilibrium point of an autonomous system $\mathbf{x}^{\prime} = f(\mathbf{x})$ (note non-zero equilibrium points can be translated to the origin). Let a function $V: U \subseteq \mathbb{R}^2 \rightarrow \mathbb{R}$ be a continously differentiable in some neighbourhood $U$ of $x^\ast$. Then, if $V$ is a positive definite function then $V$ is called as a **Lyapunov function** of the autonomous system. Furthermore, the stability of the equilibrium point $x^\ast$ is classified by the following theorem:

1) if $\frac{dV}{dt} \leq 0 \, \forall x \in U, x \neq 0$ then $\x^\ast$ is Lyapunov stable. 
2) if $\frac{dV}{dt} < 0 \, \forall x \in U, x \neq 0$ then $\x^\ast$ is asymptotically stable. 
3) if $\frac{dV}{dt} > 0 \, \forall x \in U, x \neq 0$ then $x^\ast$ is unstable. 

I will prove the first claim. Pick $\epsilon > 0$ and construct a ball $B_\epsilon: \\{x \mid x \leq \epsilon \\} \subset U$. Let $\Omega_\epsilon$ be the boundary of $B_\epsilon$. Since $V$ is continuous, it attains a minimum point $\beta$ on the boundary. Also, since $V$ is positive definite, $\beta > 0$. Construct a second ball $B_\delta$ with radius $\delta$, such that $V(x) < \beta$ for all $x \in B_\delta$. Such a $\delta$ could always be found given $V$ is positive definite (consider the levelset $V(x) = \beta$ which will be "inside" $B_\epsilon$). Consider a trajectory starting inside $B_\delta$ at the point $x_0$, i.e $\mathbf{x}(t, x_0)$. The goal is to show this trajectory can not escape $B_\epsilon$ for all $t$ since it can not cross the level set (see figure). Indeed, since $V^{\prime}(x) \leq 0$ and since $\x_0 \in B_\delta \Rightarrow V(x_0) < \beta$, this implies $V(x(t, x_0)) < \beta$ since the negative derivative means $V$ does not increase along the trajectory with initial condition $x_0$. This implies that $x(t, x_0)$ can not cross the boundary since $V(x) > \beta$ for all $x \in \Omega_\epsilon$.

This proves the first claim. The rest follow a similar argument and can be googled. Note that the third condition of the theorem is quite strong, since it is enough just one orbit to leave a neighborhood of the origin to call the equilibrium point unstable. However, the theorem is asking that all the orbits leave the neighbourhood for instability. Generalizing this fact is known as Chetaev's theorem. 

## Examples 
Examples of Lyapunov functions can be found in the accompanying blog post. 


<!-- ## Physical Interpretation of Lyapunov Functions 
Lyapunov functions have been used in various contexts (stability, convergence analysis, design of model reference adaptive systems, etc.). The Lyapunov approach is based on the physical idea that the energy of an isolated system decreases. A Lyapunov function maps scalar or vector variables to real numbers (ℜN → ℜ+) and decreases with time. The main attribute of the Lyapunov approach that makes it appealing for solving all the aforesaid engineering problems is that it is simple. The main obstacle to the use of Lyapunov theory is in finding a suitable Lyapunov function. -->