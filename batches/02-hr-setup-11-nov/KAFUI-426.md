# KAFUI-426 — Absence After Max 1440 Needs a Defined HR Rule

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-426
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** Medium
- **QA verdict:** Needs business decision and bound revision
- **Jira follow-up comment:** 10839

## الشرح بالعربي
حقل **Absence After** بيحدد بعد كام دقيقة تأخير نعتبر الشخص غائب حسب السياسة. أمير حط مدى **1–1440 دقيقة** (24 ساعة) وشرط إن Absence After يكون أكبر من Grace In.

المشكلة إن 24 ساعة حد واسع جدًا من غير تفسير من HR، ومش لازم ننسخ رقم الـGrace In/Out المقترح (30 دقيقة) على Absence After؛ لأن كل واحد ليه معنى مختلف. هنا محتاجين **HR يحدد الرقم المناسب**. المستخدم ما اختارش رقمًا نهائيًا لهذا الحقل.

## Original Jira report
**Description:** Absence After input requires sensible limits.

**Test Data:** -1, 0, 60, 120, 1440, 20000 proposed for Create/Edit.

**Expected Behavior:** Approved logical absence threshold; clear rejection outside range; preserve relevant Grace In relationship.

**Developer response (16 Nov):** Allowed 1–1440, Absence After > Grace In, API/UI validators and tests; HR approval pending.

## Retest execution — 17 Nov 2026

| Assessed condition | Observation | Verdict |
| --- | --- | --- |
| Absence After maximum | Developer bound is 1440; QA considers this policy unjustified | **BUSINESS DECISION** |
| New accepted/rejected inputs | No exact new browser values provided | **NOT CLAIMED** |

## Action Needed
Agree the business meaning and maximum of Absence After with HR, distinct from Grace In/Out. Update UI/API rules and validation copy after approval.

## Meeting question
**هل 1440 دقيقة غياب بعده معقولة في الشيفتات عندنا؟ HR معتمد كام دقيقة، وليه؟**

## History
- 11 Nov: absent-after bound issue reported.
- 16 Nov: developer set 1–1440 and flagged HR confirmation.
- 17 Nov: QA requests approved policy; ticket reopened.

**Next acceptance:** Confirmed Absence After upper limit, tested in Create/Edit.
