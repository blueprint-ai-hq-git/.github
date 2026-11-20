<div align="center">

# Blueprint AI

### Transparent AI, built from scratch.

Our own tensors, our own backward pass, our own optimiser. No PyTorch, no autograd library, no pretrained weights.
Every claim in this organisation ships with the measurement behind it — **including the measurements that killed the idea.**

[**GlassNet — the project**](https://github.com/blueprint-ai-hq-git/glassnet) · [What did not work](#what-did-not-work-and-why-it-is-published) · [Open problem](#the-open-problem-right-now)

</div>


<p align="center">
  <img src="https://raw.githubusercontent.com/blueprint-ai-hq-git/glassnet/main/docs/assets/model-card.png" alt="GlassNet model card" width="100%">
</p>

---

## What we are building

**GlassNet** is a language model whose reasoning a person can read. Not "we plotted attention" — every middle neuron carries a
name, every rule is small enough to read out loud, and `explain()` has to reconstruct the model's own answer exactly or it
refuses to present itself as an explanation.

Three properties we hold ourselves to:

| | |
|---|---|
| **From scratch** | Python standard library + NumPy. Our own `Tensor`, forward pass, gradients, loss and SGD. Every gradient is certified against finite differences before it is trusted. |
| **Measured** | No number appears without the file it came from. Paired comparisons are pin-matched on seed, window and denominator. |
| **Falsifiable** | Every experiment is launched with its prediction written down first, and a falsifier that decides when to *stop*, not when to keep tweaking. |

## Readable by construction — measured, not claimed

```
12 named per-region concepts -> 32 single-concept rules -> 10 classes
81.95% +/- 0.53 accuracy      10,166 parameters
1 of 12 concepts mislabelled  (and it is named, not hidden)
explain() verified causal: removing the top-2 named rules removes 346.7% of the logit margin,
                           removing two random rules removes 10.8%
```

Gradient certification, as it is reported in the repository:

```
age key (1,024 parameters)     worst absolute error 1.713e-09 over 71 parameters
rank-weighted loss             worst absolute error 3.366e-10 over 84 parameters
                               uniform weights reproduce the unweighted loss to 0.000e+00 (verified no-op)
```

## What did not work, and why it is published

Most projects publish only the survivors. We keep the failures in the open, with their numbers, because they are the reason
the next decision is a good one.

| Hypothesis | Result | Outcome |
|---|---|---|
| A readable window memory helps prediction | **+0.06 / +0.32 points** at the qualified 6,000-step budget (bar was +0.5) | falsifier fired, line closed |
| Positional addressing was the missing piece | age key takes synthetic delay-8 recall **20.61% → 85.61%** (3 seeds) but real-text gain **+0.14 / −0.06** | mechanism works, objective does not reward it |
| Post-hoc calibration fixes rare tokens | **+0.84 / +0.49 / +0.00** across three checkpoints | did not replicate, closed by pre-registration |
| Weighting the loss toward rare targets | **14.23% vs a 14.47% trivial constant** — below a frequency table | falsifier fired |

## The open problem right now

The model is a **top-50 predictor**. Measured across 1,100 evaluated positions:

```
ranks  21-50    25.19%      the only band that works
ranks  50-100    0.22%
ranks 100-250    0.00%
ranks 250-500    0.00%
```

Yet the correct token sits at a **median rank of 63** and is inside the model's own top-100 on **59.22%** of mid-band positions.
It knows more than it says. The current line of attack is whether the readout — not the loss — is what limits it.

## Repositories

- **[glassnet](https://github.com/blueprint-ai-hq-git/glassnet)** — the engine, the readable architecture, and the full measurement log.

## Licence & contact

MIT. Models and datasets used carry their licences in `DATA_LICENSES.md` — only MIT, Apache-2.0, CC0 and CC-BY-4.0 sources.

Project contact: **blueprintai.help1@gmail.com**
