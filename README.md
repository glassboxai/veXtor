<p align="center">
  <img src="assets/vextor-logo.png"
       alt="veXtor Logo"
       width="180">
</p>

<h1 align="center">veXtor</h1>

<p align="center">
  Controls the semantic flow of LLMs
</p>


![Status: Under Active Development](https://img.shields.io/badge/Status-Under%20Active%20Development-orange)

> 🚧 **Under Active Development**
>
> This module is currently under active development. Its architecture, features, and documentation are evolving and may change as research and implementation progress. **Not yet recommended for production use**.


## Overview

veXtor investigates whether semantic relevance can be maintained **during** autoregressive generation rather than assessed only after a response has been completed. It is part of [The Glass Box AI Project](https://glassboxai.dev/) and the experimental NeXis architecture.

The central research question is: **Can explicit semantic guidance of candidate generation states reduce topic drift while preserving answer quality, factuality, and diversity?**

## Proposed mechanism

The experimental decoding approach distinguishes the language model's token distribution from the external guidance process:

1. Obtain next-token logits from the underlying language model.
2. Convert those logits to token log-probabilities for scoring; **do not edit the logits**.
3. Expand candidate beam states and assess them using model likelihood and an external semantic-alignment signal.
4. Select or re-rank candidate beam states according to the configured guidance strategy.
5. Continue generation from the selected state or states, allowing the intervention to influence subsequent tokens.

This is **beam-state selection and reinjection**, not logit manipulation, post-generation filtering, or a guarantee that generated content is correct. The exact scoring functions and operational constraints are subject to implementation and experimental validation.

```text
User query / semantic objective
              |
              v
       Semantic guidance
              |
              v
LLM -> next-token logits -> candidate beam states
                               |
                               v
                    scoring and selection
                               |
                               v
                     selected beam states
                               |
                               +----> next decoding step
```

## Research objectives

- Measure semantic drift across long and multi-topic responses.
- Compare guided decoding with standard decoding under controlled conditions.
- Study trade-offs among relevance, coherence, diversity, latency, and token cost.
- Investigate whether intermediate guidance decisions can be made inspectable.

## Evaluation plan

Candidate baselines include greedy decoding, conventional beam search, and sampling-based decoding with matched model and prompt settings. Potential metrics include semantic similarity to the task objective, task completion, factuality assessed independently, repetition, latency, and human evaluation. Ablation studies should isolate the contribution of semantic scoring and beam-state selection.

**No benchmark results are published in this repository at present.** Improved performance is a research hypothesis, not an established outcome.

## Relationship to NeXis

Within the experimental NeXis architecture, [aXiom](https://github.com/glassboxai/aXiom) focuses on knowledge activation, veXtor on semantic guidance during generation, and [fliXer](https://github.com/glassboxai/fliXer) on explicit logical validation. These components are research directions; their integration and effectiveness require independent verification.

## Availability and roadmap

The repository currently provides a public description of the research direction. Planned public materials may include architecture notes, a reproducible prototype, tests, baseline comparisons, and experimental results. Publication dates and feature availability are not committed.

## Limitations

Semantic alignment is not equivalent to truth, logical validity, or safety. External scoring may introduce bias, reduce useful exploration, or increase computational overhead. Reproducibility depends on documenting model versions, decoding parameters, datasets, and evaluation protocols.

## Project

- [The Glass Box AI Project](https://glassboxai.dev/)
- [Glass Box AI on GitHub](https://github.com/glassboxai)

## License

This repository is licensed under **AGPL-3.0**. See [LICENSE](LICENSE). Any future source-code release will identify its applicable license and third-party dependencies.

