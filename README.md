# Pidove (Qwen3.8) vs Detect-Track-ReID: Test-Set Error Analysis

This is a diagnostic analysis on the frozen Pidove v1 Test split (31 sources, 389 queries). Static method outputs were not retuned.

## Overall

| Method | Original | Strict | Positive Strict | Negative Strict | Multi-target Strict | GT target recall |
|---|---:|---:|---:|---:|---:|---:|
| Pidove (Qwen3.8) | 161/389 (41.39%) | 152/389 (39.07%) | 137/355 (38.59%) | 15/34 (44.12%) | 0/36 (0.00%) | 146/394 (37.06%) |
| Detect-Track-ReID | 123/389 (31.62%) | 110/389 (28.28%) | 91/355 (25.63%) | 19/34 (55.88%) | 2/36 (5.56%) | 106/394 (26.90%) |

## Key findings

- Pidove leads DTR by 10.80 percentage points in Strict accuracy. The video-cluster bootstrap 95% CI is [4.50, 17.01] pp.
- DTR has stronger strict candidate coverage (73.52% vs 67.61%), while Pidove has stronger post-Grounding target selection (57.08% vs 34.87%).
- Pidove is 0/36 on strict multi-target queries, whereas DTR is 2/36.
- A diagnostic oracle selecting the better method per query reaches 196/389 (50.39%); this is not a deployable fusion result.

## Complementarity

- Both correct: 66
- Ours only: 86
- DTR only: 44
- Both wrong: 193


## Dev-selected gate

- question: Dev OOF 57/130 (43.85%), Test 152/389 (39.07%)
- video: Dev OOF 59/130 (45.38%), Test 152/389 (39.07%)
- runtime: Dev OOF 60/130 (46.15%), Test 154/389 (39.59%)

Selected on Dev: **runtime**. The result is retrospective and should be confirmed on a future untouched extension.


## Stage diagnostics

- Pidove proposals cover at least one GT: 244/355 (68.73%)
- Pidove proposals cover every GT: 240/355 (67.61%)
- DTR proposals cover at least one GT: 265/355 (74.65%)
- DTR proposals cover every GT: 261/355 (73.52%)
- Pidove succeeds when every GT is covered: 137/240 (57.08%)
- DTR succeeds when every GT is covered: 91/261 (34.87%)
- Pidove single-target strict: 137/319 (42.95%)
- DTR single-target strict: 89/319 (27.90%)

## Pidove errors

- Grounding: no GT-covered proposal: 92
- VLM selection: wrong tag: 81
- Negative: false positive: 19
- Grounding: no proposals: 19
- VLM selection: false NONE: 14
- VLM selection: incomplete multi-target output: 8
- Grounding: incomplete multi-target proposals: 4

## DTR errors

- Grounding: no GT-covered proposal: 80
- Association: wrong proposal ranking: 69
- Association: threshold rejection: 59
- ReID: person not found: 27
- Negative: false positive: 15
- Grounding: no proposals: 10
- Association: incomplete multi-target output: 10
- Association: rejected target: 5
- Grounding: incomplete multi-target proposals: 4
