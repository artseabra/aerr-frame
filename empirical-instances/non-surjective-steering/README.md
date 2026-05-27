# Non-Surjective Activation Steering

**Substrate:** geometric
**SEP instance from:** [The Ærr Trinity §5.4](../../papers/aerr-trinity/the-aerr-trinity.md#54-constructive-instances)

A Surrogate Existence Postulate (SEP) instance.

| Component | Value |
|---|---|
| Demand | A controlled behavior or state is reachable through the model's natural prompt pathway |
| Local check | White-box steering success (the intervention produces the intended local signature) |
| Cost floor | Prompt-reachability certification |
| Surrogate signature | Steering induces a state that lacks a natural prompt preimage — it lies off the prompt-reachable manifold |
| κ-positioning | Rate at which steered states are prompt-reachability certified rather than deployed as black-box equivalents |

## Reference

Mishra, A., Khashabi, D., and Liu, A. (2026). *Steered LLM Activations are Non-Surjective*. arXiv:2604.09839.

## Trinity reading

This is the cleanest geometric instance: a behavior can look right under the local steering gauge while the underlying state is off-manifold relative to natural prompting. Local success and global satisfaction diverge in coordinates the gauge cannot see.
