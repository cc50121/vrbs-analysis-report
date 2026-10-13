# Ours (Qwen3.8) vs Detect-Track-ReID: Test-Set Error Analysis

This is a diagnostic-only analysis on the frozen VRBS Test split (36 sources, 487 queries). No Test-set tuning was performed.

## Overall

| Method | Original | Strict | Positive Strict | Negative Strict | Multi-target Strict | GT target recall |
|---|---:|---:|---:|---:|---:|---:|
| Ours (Qwen3.8) | 215/487 (44.15%) | 200/487 (41.07%) | 179/444 (40.32%) | 21/43 (48.84%) | 0/48 (0.00%) | 194/495 (39.19%) |
| Detect-Track-ReID | 156/487 (32.03%) | 141/487 (28.95%) | 118/444 (26.58%) | 23/43 (53.49%) | 6/48 (12.50%) | 139/495 (28.08%) |

## Key findings

- Ours leads DTR by 12.11 percentage points in Strict accuracy. The video-cluster bootstrap 95% CI is [6.03, 18.50] pp.
- DTR has stronger strict candidate coverage (74.55% vs 70.05%), while Ours has stronger post-Grounding target selection (57.56% vs 35.65%).
- Ours is 0/48 on strict multi-target queries, whereas DTR is 6/48.
- A diagnostic oracle selecting the better method per query reaches 256/487 (52.57%); this is not a deployable fusion result.

## Complementarity

- Both correct: 85
- Ours only: 115
- DTR only: 56
- Both wrong: 231

## Stage diagnostics

- Ours proposals cover at least one GT: 316/444 (71.17%)
- Ours proposals cover every GT: 311/444 (70.05%)
- DTR proposals cover at least one GT: 336/444 (75.68%)
- DTR proposals cover every GT: 331/444 (74.55%)
- Ours succeeds when every GT is covered: 179/311 (57.56%)
- DTR succeeds when every GT is covered: 118/331 (35.65%)
- Ours single-target strict: 179/396 (45.20%)
- DTR single-target strict: 112/396 (28.28%)

## Ours errors

- Grounding: no GT-covered proposal: 104
- VLM selection: wrong tag: 102
- Grounding: no proposals: 24
- Negative: false positive: 22
- VLM selection: false NONE: 17
- VLM selection: incomplete multi-target output: 13
- Grounding: incomplete multi-target proposals: 5

## DTR errors

- Grounding: no GT-covered proposal: 95
- Association: wrong proposal ranking: 88
- Association: threshold rejection: 80
- ReID: person not found: 27
- Negative: false positive: 20
- Grounding: no proposals: 13
- Association: incomplete multi-target output: 12
- Association: rejected target: 6
- Grounding: incomplete multi-target proposals: 5
