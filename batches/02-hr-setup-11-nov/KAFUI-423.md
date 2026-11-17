# KAFUI-423 — Minimum Working Minutes Needs Approved Logical Upper Bound

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-423
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** High
- **QA verdict:** Business policy / bound not accepted as settled
- **Jira follow-up comment:** 10836

## الشرح بالعربي
أصل التذكرة إن **Minimum Working Minutes = 20000** كانت بتتحفظ، ودي قيمة غير منطقية. أمير بيقول إن الـValidation كان موجود من قبل. لكن دلوقتي إحنا بنتكلم عن **الحد الأقصى المنطقي نفسه**، مش بس رفض 20000.

الـAttendance Policy بتتعمل من غير ربط مباشر بالـShift. عشان كده ما نقدرش نفترض إن كل موظف شيفته 8 ساعات: فيه Part-time وشيفتات مختلفة. المستخدم اقترح **8 ساعات (480 دقيقة)**، وطرح احتمال **12 ساعة (720 دقيقة)** أثناء النقاش، وبعدها فضّل 480. ده **اقتراح QA** محتاج موافقة HR، مش Rule مؤكد.

## Original Jira report
**Description:** Minimum Working Minutes field accepted implausible 20000.

**Original test data:** 20000 was reported as accepted on Create; proposed 0, -1, 1440, 1441, 420 for boundary review (not all verified).

**Expected Behavior:** A sensible, documented and enforced numeric range.

**Developer response (16 Nov):** Minimum Working Minutes validation reportedly already exists; no fresh code change specified and no clear max documented.

## Retest execution — 17 Nov 2026

| What was assessed | Actual QA finding | Verdict |
| --- | --- | --- |
| Bound design and suitability | QA cannot accept an arbitrary very large daily threshold without business justification; no new exact save result was reported | **NEEDS POLICY DECISION** |
| Employee Shift relationship | Policy created before it is assigned to a Shift; a universal fixed rule may reject legitimate schedules | **OPEN BUSINESS QUESTION** |

## Action Needed
HR must explicitly choose the upper bound and define if it can vary with Shift assignment. Then enforce/document frontend/backend boundary, show clear error and report implementation evidence. Do not assume an 8-hour max is approved for all schedules.

## Meeting question
**الـMinimum Working Minutes حدها النهائي كام بالدليل؟ إزاي نختار رقم ثابت وهي Policy بتتربط بعد كده بشيفتات Part-time أو أطول؟**

## History
- **11 Nov:** Original 20000 accepted defect filed.
- **16 Nov:** Developer replied validation already implemented, issue Done.
- **17 Nov:** QA requests a defined and approved logical upper bound; reopened for business ruling and QA evidence.

**Next acceptance:** Explicit approved limit, actual Create/Edit error behavior and documented decision.
