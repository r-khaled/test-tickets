# KAFUI-425 — Grace In / Grace Out Max 1440 Is Too Permissive

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-425
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** Medium
- **QA verdict:** Needs grace-limit revision / HR approval
- **Jira follow-up comment:** 10838

## الشرح بالعربي
الـGrace In والـGrace Out هما هامش سماح للتأخير أو الخروج. أمير حط الـMaximum بتاع كل واحد **1440 دقيقة**، يعني يوم كامل! ده غير مناسب كهامش سماح يومي عادي. اقتراحنا إن **كل واحد Maximum = 30 دقيقة**، مع اعتماد HR قبل التنفيذ.

## Original Jira report
**Description:** Grace In and Grace Out lacked logical documented upper/lower bounds.

**Test Data:** -1, 0, 15, 1440, 20000 for Grace In; -1, 0, 10, 1440, 20000 for Grace Out.

**Expected Behavior:** Independent, sensible limits and field-specific error messages on Create/Edit.

**Developer response (16 Nov):** API/UI max 1440 for both, plus Grace In < Absence After; unit/integration tests claimed; HR approval pending.

## Retest execution — 17 Nov 2026

| Assessed condition | Observation | Verdict |
| --- | --- | --- |
| Business appropriateness of maximum | 1440 minutes is not a meaningful grace window under proposed normal attendance policy | **POLICY REVISION** |
| Browser acceptance of exact boundary values | No new exact save test data recorded | **NOT CLAIMED** |

## Action Needed
Have HR approve separate Grace In/Out bounds, with **30-minute maximum for each** as current QA proposal. Update UI/API range and messages. Avoid conflating grace limits with Absence After threshold.

## Meeting question
**إزاي تسمح Grace In أو Grace Out بمهلة يوم كامل؟ ليه الـGrace ما تبقاش في نطاق عملي بحد أقصى 30 دقيقة بعد اعتماد HR؟**

## History
- 11 Nov: original field-limit issue.
- 16 Nov: developer applied 1440 and flagged HR approval.
- 17 Nov: QA requests policy change; ticket reopened.

**Next acceptance:** HR-approved grace limits implemented and verified.
