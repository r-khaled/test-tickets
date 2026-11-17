# KAFUI-414 — Unsaved Changes Warning Missing in Redesigned Work Schedules

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-414
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** High
- **QA verdict:** PARTIAL PASS; FAILED on redesigned Work Schedules
- **Jira follow-up comment:** 10834

## الشرح بالعربي
كنا طالبين تحذير قبل ما المستخدم يقفل أي Add/Edit Form وهو كاتب بيانات ولسه ما عملهاش Save. **الـPolicy دي اشتغلت بالفعل في الـHR Setup العادية**، والـQA أكد ده في مراجعة 17 نوفمبر، لكن شاشة **Work Schedules** اتغير تصميمها بالكامل والتحذير ما اتطبقش عليها. ما ينفعش نقفل التذكرة كأن السياسة شغالة على كل الشاشات.

**مشكلة ثانية اتلاحظت في نفس الشاشة:** لو تضغط Save وفيه حقول مطلوبة ناقصة أو قيم غلط، الزر مش بيكمل ومفيش Validation Errors توضّح الغلط. دي مشكلة مستقلة عن حماية التعديلات غير المحفوظة، وهنعمل لها Jira ticket منفصلة بعد الموافقة.

## Original Jira report
**Description:** HR Setup Create/Edit screens discard unsaved user changes without a confirmation prompt.

**Test Data:** On Add New/Edit for HR Setup, edit one or more fields and use Cancel/Back/navigation without saving. Exact original input strings not recorded.

**Expected Behavior:** Show **Discard changes and leave / Continue editing** (or equivalent); preserve entered values on Continue Editing; do not warn on clean forms.

**Real Behavior (original):** Input was lost without warning.

**Developer response (16 Nov):** The developer claimed the feature already existed in code, with no new change.

## Retest execution — 17 Nov 2026

| Action | Expected | Observed | Verdict |
| --- | --- | --- | --- |
| Edit ordinary HR Setup form then leave | Unsaved changes confirmation appears | The shared behavior is now working | **PASS** |
| Enter unsaved values in redesigned Work Schedules Add/Edit then leave | Same confirmation appears | Warning not applied in redesigned screen | **FAIL** |

**Data/environment:** Work Schedules redesigned UI; exact record identifiers and input values not supplied.

## Action Needed
Apply unsaved/dirty form confirmation to **redesigned Work Schedules Create/Edit** with the same policy as other HR Setup forms. Developer should identify which change was deployed and demonstrate the confirmation in browser.

## Meeting question
**ليه الـUnsaved Changes Policy اتطبقت على الـHR Setup العادية، لكن شاشة Work Schedules الجديدة ما ورثتش نفس الحماية بعد إعادة تصميمها؟**

## History
- **11 Nov:** Initial report: HR Setup form unsaved data not protected.
- **16 Nov:** Developer marked Done, claimed it was already implemented.
- **17 Nov:** Tester confirmed general behavior PASS, exception on redesigned Work Schedules FAIL. Jira reopened and assigned to Shaimaa Kamal.

**Next QA acceptance:** Warning works when leaving dirty Work Schedules Create/Edit and can return to editing without losing entered values.

**Scope separation:** The missing required-field validation is a separate proposed issue under HR Setup, related to this one; do not merge the two defects.
