# KAFUI-427 — Minimum Working Minutes vs Shift Net Duration: HR Decision Pending

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-427
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** Medium
- **QA verdict:** BUSINESS CLARIFICATION OPEN — not a proven code bug
- **Jira follow-up comment:** 10840

## الشرح بالعربي
الـMinimum Working Minutes في الـAttendance Policy **مش لازم تساوي مدة الـShift**. ممكن الشيفت من 8 الصبح إلى 5 مساءً وفيه ساعة Break، يبقى صافي الشيفت 8 ساعات، ومع ذلك سياسة قبول الحضور ممكن تعتبر 7 ساعات كفاية، حسب قواعد الشركة. كمان Part-time ممكن يكون أقل.

المشكلة إن الـPolicy بتتعمل لوحدها، وبعد كده تتربط بموظف/Shift. فمش صح نفرض Equality وقت إنشاء Policy. لكن لسه محتاجين نتفق هل قيمة Minimum Working Minutes ممكن تبقى **أعلى من صافي مدة الشيفت الفعلي**، ولو لأ، التحقق ده يتحط فين.

**اقتراح المستخدم:** حد أقصى عام 8 ساعات (480 دقيقة)، لكن ده ما اتعتمدش رسميًا وممكن يحتاج استثناء للشيفتات الطويلة. ما نعتبرش الاقتراح Business Rule مؤكدة.

## Original Jira report
**Description:** Determine how Attendance Policy Minimum Working Minutes interacts with Shift Minimum Working Hours, planned net Shift time, and different employee assignments.

**Original test data:** Shift 08:00–17:00 minus 60-minute break = 480 net minutes; proposed policy threshold 420; shorter 300-minute shift for comparison.

**Expected Behavior:** Confirm equality vs tolerance; validation location (assignment/calculation) and precedence when employee/weekday shifts differ.

**Developer response (16 Nov):** Independent acceptance threshold, can be less than planned net Shift, same policy reusable across shifts, no strict equality; HR approval pending.

## Execution record — 17 Nov 2026

| Decision | QA view | Status |
| --- | --- | --- |
| Must Minimum Working Minutes equal net Shift? | No, lower threshold may be valid | **QA AGREES / HR TO CONFIRM** |
| May threshold exceed actual net Shift? | Should be considered at assignment/calculation, not blindly at standalone create | **UNRESOLVED** |
| Universal max 480 minutes? | QA proposes, but part-time/longer shifts and actual business policy matter | **UNAPPROVED PROPOSAL** |

No new live UI contradiction or test values were supplied; this is **Business Clarification**, not a newly confirmed validation defect.

## Action Needed
HR/Product owner decides permitted policy maximum and compatibility with a specific assigned Shift. Document the rule and only then implement required validation in a context that knows the actual Shift.

## Meeting question
**إحنا متفقين إن Minimum Working Minutes مش شرط تساوي الشيفت. لكن هل ينفع تزيد عن صافي الشيفت؟ ولو ممنوع، فين بيتعمل التحقق بعد ربط الـPolicy بالـShift؟**

## History
- 11 Nov: rule ambiguity ticket created.
- 16 Nov: developer documented independent threshold, pending HR sign-off, marked Done.
- 17 Nov: QA leaves clarification unresolved and reopens to Shaimaa.

**Next acceptance:** Signed-off business decision and matching implementation/behavior if required.
