# Ours (Qwen3.8, LA+GD) vs Detect-Track-ReID: Pidove Test-Set Error Analysis

This is a diagnostic analysis on the frozen Pidove v1 Test split (31 sources, 389 queries). Static method outputs were not retuned.

## Overall

| Method | Original | Strict | Positive Strict | Negative Strict | Multi-target Strict | GT target recall |
|---|---:|---:|---:|---:|---:|---:|
| Ours (Qwen3.8, LA+GD) | 170/389 (43.70%) | 156/389 (40.10%) | 144/355 (40.56%) | 12/34 (35.29%) | 0/36 (0.00%) | 158/394 (40.10%) |
| Detect-Track-ReID | 123/389 (31.62%) | 110/389 (28.28%) | 91/355 (25.63%) | 19/34 (55.88%) | 2/36 (5.56%) | 106/394 (26.90%) |

## Key findings

- Ours leads DTR by 11.83 percentage points in Strict accuracy. The video-cluster bootstrap 95% CI is [5.23, 18.35] pp.
- Both methods use LocateAnything and Grounding DINO with their own frozen proposal settings. Strict proposal coverage is 75.49% for Ours and 73.52% for DTR; their post-Grounding success rates are 53.73% and 34.87%, respectively.
- Ours is 0/36 on strict multi-target queries, whereas DTR is 2/36.
- A diagnostic oracle selecting the better method per query reaches 198/389 (50.90%); this is not a deployable fusion result.

## Complementarity

- Both correct: 68
- Ours only: 88
- DTR only: 42
- Both wrong: 191




## Detector fusion ablation

- LocateAnything: Dev Original 63/130 (48.46%), Dev Strict 57/130 (43.85%); Test Original 161/389 (41.39%), Test Strict 152/389 (39.07%)
- LocateAnything + Grounding DINO: Dev Original 69/130 (53.08%), Dev Strict 63/130 (48.46%); Test Original 170/389 (43.70%), Test Strict 156/389 (40.10%)

LA+GD improves Dev by +4.62 pp Original / +4.62 pp Strict and Test by +2.31 pp Original / +1.03 pp Strict.


## Stage diagnostics

- Ours proposals cover at least one GT: 276/355 (77.75%)
- Ours proposals cover every GT: 268/355 (75.49%)
- DTR proposals cover at least one GT: 265/355 (74.65%)
- DTR proposals cover every GT: 261/355 (73.52%)
- Ours succeeds when every GT is covered: 144/268 (53.73%)
- DTR succeeds when every GT is covered: 91/261 (34.87%)
- Ours single-target strict: 144/319 (45.14%)
- DTR single-target strict: 89/319 (27.90%)

## Ours errors

- VLM selection: wrong tag: 103
- Grounding: no GT-covered proposal: 79
- Negative: false positive: 22
- VLM selection: false NONE: 11
- VLM selection: incomplete multi-target output: 10
- Grounding: incomplete multi-target proposals: 8

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
