# KAFUI-424 — Rounding Minutes Max 1440 Is Not a Logical Increment

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-424
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** Medium
- **QA verdict:** Needs limit revision / HR business confirmation
- **Jira follow-up comment:** 10837

## الشرح بالعربي
الـRounding Minutes دي وحدة تقريب للحضور والتأخير. أمير حط لها **Maximum = 1440 دقيقة = 24 ساعة**. حتى لو دي بتمنع رقم 20000، 24 ساعة **مش حد منطقي لخطوة تقريب**. اقتراح QA إن الحد الأقصى يكون **30 دقيقة**، لكن HR لازم يقرر النطاق والخطوات المسموحة.

## Original Jira report
**Description:** Rounding Minutes missing explicit logical input bounds.

**Test Data:** -1, 0, 1, 15, 60, 1440, 20000 suggested on Create/Edit; individual original save results not fully verified.

**Expected Behavior:** Define reasonable min/max and permitted increments; reject unsupported values with clear field error.

**Developer response (16 Nov):** API and frontend max = **1440**, with automated tests; needs HR approval.

## Retest execution — 17 Nov 2026

| Assessed condition | Observation | Verdict |
| --- | --- | --- |
| Rounding Minutes current upper-limit policy | Developer-stated ceiling 1440 is too permissive as a business rule | **POLICY REVISION** |
| New boundary-save attempts | Tester did not provide exact accepted/rejected values this review | **NOT CLAIMED** |

## Action Needed
Ask HR to approve QA's proposed **30-minute maximum** and allowed increments (including whether 0 disables rounding). Apply approved limit in UI/API and demonstrate on Create/Edit.

## Meeting question
**ليه بنسمح إن Rounding Minutes تصل 24 ساعة؟ إيه الاستخدام التجاري اللي يبرر الحد ده بدل حد عملي زي 30 دقيقة؟**

## History
- 11 Nov: field-limits issue.
- 15 Nov: Shaimaa asked for permitted range; tester requested logical boundaries.
- 16 Nov: Developer applied 1440.
- 17 Nov: QA rejected this as insufficient business policy; Jira reopened.

**Next acceptance:** HR-approved sensible max and tested Create/Edit validation.
