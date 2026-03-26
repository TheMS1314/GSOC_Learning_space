# Emperor's Dilemma — Extended

## What this model does

The Emperor's Dilemma simulates how unpopular norms sustain themselves 
through fear and false enforcement. Most agents privately reject the norm 
but publicly comply — and even enforce it on others — because they assume 
everyone else believes in it. Nobody does. That's the trap.

I chose this model because it connects directly to something I think about 
philosophically — how societies end up loudly defending things nobody 
actually believes in. The model formalizes that intuition mathematically.

## What I added

The base model already tracked compliance, enforcement, and false 
enforcement. I extended it with three things:

**WhistleblowerAgent** — a new agent subclass that is immune to social 
pressure. Regular agents cave when neighbours enforce hard enough. 
Whistleblowers always act on their private beliefs regardless. 
They've decided they're done pretending.

**Belief Gap metric** — tracks the average distance between private belief 
and public behaviour across all agents at each step. When this is high, 
the society is wearing a collective mask. When it drops to zero, 
everyone is acting honestly.

**Norm collapse detection** — records exactly which step compliance first 
drops below 50%. Turns a visual observation into a measurable data point.

## What Mesa features this uses

- `FixedAgent` and subclassing for the WhistleblowerAgent
- `OrthogonalMooreGrid` for the spatial structure
- `DataCollector` with custom model reporters
- `SolaraViz` with `SpaceRenderer` and `make_plot_component`
- `agents.shuffle_do` for random activation order

## What I learned

The hardest part was understanding why relative imports break when running 
with `solara run app.py` directly. The fix was switching from `.model` to 
`model` — but understanding *why* that happens took me a while. 
Solara runs the file as a script, not as part of a package, so there's 
no parent package for the relative import to resolve against.

The most interesting thing I learned was from actually running the model. 
I expected whistleblowers to gradually weaken the norm as their fraction 
increased. Instead, there's a sharp tipping point — at 0.02 the norm holds 
completely, at 0.05 it weakens but survives, at 0.10 it collapses entirely. 
That threshold behaviour wasn't something I designed in — it emerged from 
the interaction rules. That's what makes ABM interesting.

## What I'd do differently

I'd add a parameter sweep to find the exact tipping point systematically 
rather than manually dragging the slider. Something like running the model 
100 times at each whistleblower fraction from 0 to 0.15 in steps of 0.01 
and plotting the average collapse step. That would give a proper phase 
transition curve.

## How to run
```bash
cd models/emperor_dilemma
solara run app.py
```

## Things to try

- Set whistleblowers to 0 — this reproduces the original model exactly
- Slowly increase whistleblowers from 0 and watch the tipping point
- Try homophily=True with low whistleblowers — does clustering help or hurt?
- Lower enforcement cost K and see how that interacts with whistleblowers
