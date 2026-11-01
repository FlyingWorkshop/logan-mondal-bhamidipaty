---
title:  "Imperfect World Models are Exploitable"
date:   2026-05-20 11:54:37 -0800
authors: <strong>Logan Mondal Bhamidipaty</strong>, Esmeralda S. Whitammer, David Abel, Mykel Kochenderfer, Subramanian Ramamoorthy
category: research
image: model_exploitation.gif
venue: RL in Big Worlds Workshop at the Reinforcement Learning Conference (RLC)
paper: https://arxiv.org/abs/2605.15960
poster: assets/pdfs/wm_theory_poster.pdf
---
We call a world model exploitable when it ranks one policy above another while the true dynamics rank them in reverse, an ordinal and global notion of model safety rather than the usual cardinal one based on prediction error. This view yields a unified geometric theory of reward hacking and model exploitation that recovers earlier hacking results as a special case, and it shows that exploitation is essentially unavoidable once the policy set is large enough. We then derive a safe horizon, a bound on the effective planning horizon under which unexploitability is guaranteed.
