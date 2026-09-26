# QA plan + quality gates

This plan is for the traffic-light annotation project and uses the downstream contract in `01_problem_statement.md`. Thresholds are proposed for this small class exercise; they are not industry standards.

## Flow

Guideline → independent calibration → annotation → self-QC → risk-based review → rework → quality gate.

- **Reviewers:** Nguyễn Thị My (`@nguyenmy133`) coordinates QA; rotate independent review among team members, and annotators do not review their own samples. The gold owner checks every blind decision and geometry item.
- **Review volume:** review 100% of blind-set decisions and all cases tagged `critical`, `ambiguity`, `occlusion`, or `low_visibility`. For other annotated images, review at least 20%, with a minimum of one image per annotator.
- **Calibration entry check:** compare only exports from the same CVAT task and label schema that contain exactly the calibration IDs in `sample_pack.csv`. Keep nonmatching exports out of the calibration report and request a corrected export.
- **Sample selection:** choose risk-tagged samples first, then select the remaining review images across annotators and scene/time-of-day strata. Record the reason for every selected sample.
- **Issue log:** record each defect in `05_qa_issues.csv` with sample, object, severity, evidence, owner, action, and status. A reviewer closes an issue only after checking the corrected export against the same rule.
- **Guideline gaps:** add the rule or escalation path, update the guideline version, record the affected sample and evidence in `08_revision_log.md`, then re-review every affected sample. Do not silently resolve a domain disagreement in chat.

## Defect severity

| Severity | Definition for this project | Example | Default action |
|---|---|---|---|
| Critical | A defect can reverse the safe action for the ego lane. | Missed ego-lane red signal; wrong `relevance` that makes an ego signal appear to control another lane. | Stop handoff, correct all affected samples, and re-review the full risk slice. |
| Major | A defect changes the object or its operational attribute, but does not meet the critical definition. | Missed/extra signal head; wrong `state` or `shape`; box edge is more than 5 px outside the housing, cuts visible housing, or merges separate heads. | Rework the sample and review adjacent decisions using the same rule. |
| Minor | A geometry defect exceeds the 2 px tolerance by no more than 5 px and leaves the signal head and its interpretation intact. | One box edge is 3–5 px outside the visible housing boundary. | Correct the box; record if the pattern repeats. |
| Question | Available evidence or written rules do not support a consistent decision. | A head is visible but the controlled lane cannot be established. | Use `relevance = ambiguous`; log the question and revise or escalate the rule. |

## Metrics

| Metric | Calculation | Why it fits this project |
|---|---|---|
| Attribute exact-match rate | Correct reviewed values for `state`, `relevance`, and `shape` divided by reviewed attribute decisions. | Attribute errors change whether and how the vehicle should respond to a signal. |
| Geometry pass rate | Reviewed boxes with every visible-housing edge within 2 px divided by reviewed boxes. | The guideline defines an explicit edge tolerance. |
| Critical escape rate | Critical defects remaining after rework divided by critical defects found before rework. | Any remaining ego-lane signal error can cause a high-consequence downstream failure. |

Review all critical-risk decisions; do not estimate this metric from a random sample.

## Quality gate

```text
PASS if:
  critical escape rate = 0 after rework
  attribute exact-match rate >= 95% on the reviewed batch
  geometry pass rate >= 95% on the reviewed batch
REWORK if: any metric misses its threshold and the decision can be corrected from the current guideline.
REJECT / ESCALATE if: a critical decision remains wrong, evidence is insufficient, or annotators cannot reach one decision from the written rule.
```

**Trade-off:** the sample pack is small, so risk slices receive complete review while a 20% floor keeps normal cases represented without spending the whole lab on review. Zero critical escapes is intentionally strict because the stated use case includes automated braking and behavior planning. The 95% noncritical thresholds allow limited execution mistakes while requiring correction and a documented cause.
