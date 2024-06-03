+++
title = "AI Chess Engine"
description = "An AI which uses Q-learning and self-play to discover novel chess tactics."
weight = 2

[extra]
local_image = "ai-chess-engine.jpg"
+++


## Static learning approach
- $\mathcal{B}$ Set of board states.
- $\mathcal{M_{b}}$ Set of legal chess moves on board $b\in\mathcal{B}$.
- $\mathcal{M_{b}^\ast}$ Set of capturing moves on board $b\in\mathcal{B}$, such that $\mathcal{M_{b}^\ast}\subset\mathcal{M_{b}}$.
- $\mathcal{P}$ Set of pieces on a chess board.
- $U: \mathcal{P}\to \mathbb{R}$ The value of a certain chess piece.
- $P^*: \mathcal{M_{b}^\ast}\to \mathcal{P}$ Piece captured by move $\mathcal{M_{b}^\ast}$.
- $R: \mathcal{M_{B}^\ast}\to \mathbb{R} \equiv U \circ P^\ast$ Reward gained from a certain capturing move.
- $V: \mathcal{B}\to \mathbb{R}$: Value of a certain board state.

The next move of the player given board $b\in\mathcal{B}$ is decided by:

$$\widetilde{m}(b)=\underset{ m\in\mathcal{M}_{b}}{\text{argmax}}\left\lbrace R(m) + \gamma V(b')\right\rbrace$$

Where $b'$ is the board achieved by move $m$ on board $b$, and $\gamma$ is the discount factor. The value function of a board is recursively defined by:

$$V(b)= \underset{m\in\mathcal{M}_{b}}{\text{max}} \left\lbrace R(m) + \gamma V(b') \right\rbrace$$

The value function and the piece utility function can be approximated by two neural networks, parameterised by $\theta$ and $\phi$:

$$\widehat{V}\_{\theta}(b)\simeq \underset{m\in\mathcal{M}\_b}{\text{max}} \left\lbrace \widehat{U}\_{\phi}(P^\ast(m)) + \gamma \widehat{V}_{\theta}(b')\right\rbrace$$
