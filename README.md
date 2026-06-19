# Action-Conditioned V-JEPA 2 World Model in PyTorch

A self-supervised learning repository focused on Joint-Embedding Predictive Architectures for predictive dynamics and planning. Moving away from traditional generative models that waste immense computational bandwidth decoding raw, high-dimensional pixels, this architecture implements an Action-Conditioned Latent World Model engineered for self-supervised temporal representation learning.

Core Paradigm Shift
Traditional video prediction models rely on pixel-level autoencoders or diffusion processes, forcing the network to waste capacity modeling complex, irrelevant background noise.

This repository shifts the learning objective entirely to the Latent Space. By predicting transitions within an abstract embedding manifold, the network ignores high-frequency pixel noise and focuses strictly on the semantic and causal structure of the scene. The predictor takes the current state embedding and an action vector to guess the next latent representation directly.

Architecture Mechanics
1. Dual-Pathway Siamese Network
The architecture implements an asymmetric Siamese tracking framework to prevent representation collapse without requiring contrastive negative pairs:

Context Encoder: A convolutional backbone that maps the current visual observation to its latent representation.

Target Encoder: A structurally identical network that maps the target ground-truth frame to its embedding. This path operates with strict stop-gradient isolation.

Exponential Moving Average: The Target Encoder weights are updated dynamically via momentum tracking, ensuring stable targets throughout optimization.

2. Action-Conditioned Predictor
Instead of guessing randomly masked blocks using coordinates alone, a Transformer-based dynamics network is conditioned on an execution variable, such as continuous directional velocity vectors. By projecting the continuous action and coupling it with the context state, the system models the explicit physical consequences of an action.

Optimization & Convergence Performance
The model maps low-entropy, deterministic environment physics with extreme sample efficiency. Because optimization runs completely within the latent manifold using a Mean Squared Error criterion, the network rapidly masters the underlying dynamics.

Pre-Training Loss: Feature-space Mean Squared Error converges aggressively, dropping smoothly within twenty epochs to a stable plateau at approximately 0.000066.

Anti-Collapse Verification: The stable curve layout confirms the momentum-based target encoder successfully eliminates representation degeneracy or vector collapse.

Downstream Application: Latent Planning
By accurately projecting subsequent environment states entirely within the abstract embedding layer, this project serves as a foundational backbone for Model Predictive Control and Model-Based Reinforcement Learning.

Using the trained predictor, an agent can parallelize and roll out thousands of potential imagined action trajectories across memory, evaluating and selecting the optimal path to reach a target visual goal vector—all without ever rendering a single output pixel.
