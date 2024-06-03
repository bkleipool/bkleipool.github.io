+++
title = "Rocket Trajectory Prediction"
description = "A new method of predicting the trajectory of a rocket."
weight = 1

[extra]
local_image = "rocket-downrange.jpg"
+++

## Main idea
In rocket trajectory simulation, you're basically doing a "particle" simulation of a discrete number of particles to know where they may end up after a certain time. You could do this in a continuum by propagating a probability distribution of where the particle could be over time.

## Motivation

## Technical Details

### Derivation of PDE
Symbols:
- $\mathbf{X}(t)$: a state vector in the state space of the system.
- $P(\mathbf{X}, t)$: the probability density of observing a state $\mathbf{X}$ at time $t$. 
- $\mathbf{f}(\mathbf{X}, t)$: deterministic system response.
- $\mathbf{g}(\mathbf{X}, t)$: stochastic system response.
- $\Delta x_{i}, \Delta t$: Discrete steps in state and time the computation domain.
- $P_{i,j,\dots}^k$: probability of observing a discrete state $X_{i,j,\dots}$ at timestep $t_{k}$.


Deterministic differential equation of the state vector:
$$\dot{\mathbf{X}}=\mathbf{f}(\mathbf{X}, t)  \qquad (1)$$
By introducing a bit of noise (wind gusts, etc) in the form of a multivariate Wiener process $\mathbf{W}$, we get a stochastic differential equation of the state vector:
$$d\mathbf{X}=\mathbf{f}(\mathbf{X}, t)dt + \mathbf{g}(\mathbf{X}, t)\odot d\mathbf{W} \qquad (2)$$
Applying the Fokker-Planck equation to $(2)$:

$$\frac{\partial P(\mathbf{X}, t)}{\partial t} =-\frac{\partial}{\partial \mathbf{X}}\left[\mathbf{f}(\mathbf{X}, t)P(\mathbf{X}, t)\right] + \frac{\partial^2}{\partial \mathbf{X}^2}\left[\frac{1}{2}\mathbf{g}^2(\mathbf{X}, t)P(\mathbf{X}, t)\right]$$

### Finite Volume Discretisation
Conservation of probability means that each discrete state-space volume $\Omega$ has a time-change in probability equal to the probability flux along the boundary $\partial \Omega$:
$$\frac{\partial}{\partial t} \int_{\Omega} P(\mathbf{X}, t)\,dV=-\oint_{\partial\Omega} \mathbf{J}(\mathbf{X}, t)\cdot d\mathbf{S}$$
The Fokker-Planck PDE can be written in conservative form as:
$$\frac{\partial P(\mathbf{X}, t)}{\partial t} = \nabla_{\mathbf{x}}^2\left[\tfrac{1}{2}\mathbf{g}^2(\mathbf{X, t})P(\mathbf{X}, t) \right] - \nabla_{\mathbf{x}}\cdot[\mathbf{f}(\mathbf{X}, t)P(\mathbf{X}, t)]$$
We can derive the finite-volume form of the conservative equation:
$$
\int_{\Omega}\frac{\partial P(\mathbf{X}, t)}{\partial t}\,dV = \int_{\Omega}\nabla_{\mathbf{x}}^2\left[\tfrac{1}{2}\mathbf{g}^2(\mathbf{X}, t)P(\mathbf{X}, t) \right]\,dV - \int_{\Omega}\nabla_{\mathbf{x}}\cdot[\mathbf{f}(\mathbf{X}, t)P(\mathbf{X}, t)]\,dV$$
$$\frac{\partial}{\partial t}\int_{\Omega}P(\mathbf{X}, t)\,dV = \oint_{\partial\Omega}\nabla_{\mathbf{x}}\left[\tfrac{1}{2}\mathbf{g}^2(\mathbf{X}, t)P(\mathbf{X}, t) \right]\cdot d\mathbf{S} - \oint_{\partial\Omega}(\mathbf{f}(\mathbf{X}, t)P(\mathbf{X}, t))\cdot d\mathbf{S}$$
$$\frac{\partial}{\partial t}\int_{\Omega}P(\mathbf{X}, t)\,dV = \oint_{\partial\Omega}\left(\tfrac{1}{2}\nabla_{\mathbf{x}}\left[\mathbf{g}^2(\mathbf{X}, t)P(\mathbf{X}, t) \right] - \mathbf{f}(\mathbf{X}, t)P(\mathbf{X}, t)\right)\cdot d\mathbf{S}$$

In a discrete hypercube in $N$-dimensional state-space ($V$ is the cell volume, $S$ is the area of every wall of the cell):

Discretizing the spatial derivative as the difference between neighboring volumes:

