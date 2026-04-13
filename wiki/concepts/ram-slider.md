---
title: RAM Slider Bar
created: 2026-04-07
updated: 2026-04-13
tags: [artifact, RAM, slider, mouse-tracking, response-cost]
source: raw/google-slides/Sharon Research Meeting-2026-04-07.pdf
---

# RAM Slider Bar

## Definition

A UI intervention tool designed based on the Response Activation Model (RAM), which suppresses exaggeration behavior by increasing the "physical interaction cost" of dishonest reporting.

## Design Theory (RAM Theory)

Cited from Yu (2023), based on Welsh & Elliott (2004) and Weinmann et al. (2021):

> "When there is intention to misreport, individuals might hesitate between multiple potential target locations, resulting in deviations from the simplest and swiftest possible movement."

Therefore, by increasing the physical interaction cost to reach a dishonest target location, users perceive the required effort before acting, forcing them to confront their dishonest intention before executing the action.

## Three Starting Position Designs

| Starting Position | Design Intent | Theoretical Rationale |
|-------------------|---------------|-----------------------|
| Far left (least healthy) | Experimental group (increases interaction cost) | Reporting a high score requires more movement; RAM effect is strongest |
| Middle | Control group | No bias |
| Far right (most healthy) | Reverse control | Users do not need to "overcome" their dishonest intention |

## Dependent Variables (Mouse-Tracking Metrics)

| Metric | Definition | Honest vs. Dishonest |
|--------|------------|----------------------|
| Diet Score | Reported health score | Honest → lower (closer to true value) |
| Response Speed | Speed of answering | Ruby's study: faster → more honest |
| Response Correction | Number of slider position changes | More → more conflict → potentially dishonest |
| Response Gap | max(Score_t-1 − Score_t) | Larger → possibly deliberate inflation |

## Comparison with Ruby's Study

| Item | Ruby | Sharon |
|------|------|--------|
| RAM starting position | Least-healthy end | Least-healthy end (same) |
| Message intervention | Fear Appeal | Promotion-focus × Self-efficacy |
| Primary tracking metric | Speed | Correction + Gap + ML classification |

## Integration with Mouse Tracking

The slider trajectory (x-position × timestamp) collected by the RAM slider is directly related to Weinmann (2022)'s mouse tracking research:
- More deviation from shortest path → cognitive conflict (cognitive dissonance/load)
- Slower speed → Cognitive Load Theory

## See Also
- [[wiki/concepts/mouse-movement-metrics.md]]
- [[wiki/concepts/promotion-efficacy-fit.md]]
- [[wiki/references/weinmann-2022.md]]
