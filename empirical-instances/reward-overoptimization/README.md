# Reward Model Overoptimization

**Substrate:** behavioral
**SEP instance from:** [The Ærr Trinity §5.4](../../papers/aerr-trinity/the-aerr-trinity.md#54-constructive-instances)

A Surrogate Existence Postulate (SEP) instance.

| Component | Value |
|---|---|
| Demand | Output genuinely preferred by humans |
| Local check | Proxy reward score |
| Cost floor | Reliable preference certification (human evaluation or a robust gold reward model) |
| Surrogate signature | Proxy reward rises while gold reward declines |
| κ-positioning | Rate at which the pipeline performs gold-reward certification rather than accepting the proxy |

## Reference

Gao, L., Schulman, J., and Hilton, J. (2023). Scaling laws for reward model overoptimization. *Proceedings of the 40th International Conference on Machine Learning*, PMLR 202:10835–10866. [arXiv:2210.10760](https://arxiv.org/abs/2210.10760)

## Trinity reading

This is a Goodhart instance read at the floor, discipline, and drift levels. ICL identifies the floor (gold reward certification); κ measures the rate at which it gets paid; the Ærr Rate measures how fast pipelines drift when it doesn't. See also the paper's §11 distinction from neighboring claims.
