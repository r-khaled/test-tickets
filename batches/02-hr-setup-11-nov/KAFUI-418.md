# KAFUI-418 — Recurring Holiday Displays Original Year Under Future-Year Filter

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-418
- **Reporting Batch:** 02 — HR Setup, 11 November 2026
- **Current Jira:** To Do — reopened 17 November 2026
- **Assigned:** Shaimaa Kamal
- **QA reference:** 17 November 2026 manual tester review
- **Original priority:** Medium
- **QA verdict:** Recurrence PASS; date presentation follow-up / business confirmation
- **Jira follow-up comment:** 10835

## الشرح بالعربي
بعد تعديل Year Filter، لو عندنا Holiday متكررة من 2026 واخترنا فلتر سنة **2028**، السيستم دلوقتي بيجيب العطلة — وده **صح**. لكن في صف الـHoliday بيظهر التاريخ بالسنة القديمة **2026**، فيخلي المستخدم يفتكر إن العطلة تخص السنة القديمة رغم إنه عامل فلتر 2028.

اقتراحنا: إما تعرض **تاريخ حدوثها في السنة المختارة** (2028) أو تعرض **اليوم والشهر بس** للعطلات السنوية المتكررة. اختيار طريقة العرض محتاج تأكيد Business. مش مطلوب تعديل التواريخ المخزنة يدويًا لكل سنة.

## Original Jira report
**Description:** Recurring checkbox semantics, annual Gregorian versus Hijri recurrence and behavior across year/date filters were unclear.

**Test Data:** Annual recurring fixed-date Gregorian holiday originally dated 11/11/2026; Year filter 2027/2028; separate nonrecurring holiday for comparison. This illustrative fixed date comes from the original ticket.

**Expected Behavior:** Recurring Gregorian holiday applies annually from its starting year; nonrecurring holiday only in its own year; Hijri recurrence policy requires business confirmation.

**Developer response (16 Nov):** Annual Gregorian recurrence on same month/day, Hijri holidays entered separately per year; HR policy approval needed.

## Retest execution — 17 Nov 2026

| Action | Expected | Observed | Verdict |
| --- | --- | --- | --- |
| Filter a future year with recurring and nonrecurring holidays | Relevant recurring and matching-year nonrecurring results displayed | Filtering now works; recurring holiday appears | **PASS** |
| Inspect displayed Date of recurring holiday under future-year filter | Date must clearly represent selected occurrence or use an unambiguous month/day format | Date still shows original 2026 under a 2028 filter | **FOLLOW-UP UX ISSUE** |

**Observed example:** original-year 2026, selected-year 2028. Exact holiday record ID not supplied.

## Action Needed
Product/HR approve whether a recurring holiday row should show **the occurrence in the selected year** or **day/month without year**, then correct the list display. Preserve filter/recurrence matching. Keep the Hijri versus Gregorian policy explicitly approved/documented.

## Meeting question
**ليه فلتر 2028 بيعرض Holiday recurring صح لكن بتاريخ 2026 جوه الصف؟ هنختار عرض تاريخ الحدوث في السنة المطلوبة ولا اليوم والشهر بس؟**

## History
- **11 Nov:** Recurrence and year-filter questions reported.
- **16 Nov:** Developer documented Gregorian annual recurrence; requested HR approval.
- **17 Nov:** QA verifies matching PASS, but display year misleading; reopened to Shaimaa.

**Next QA acceptance:** Selected-year recurring holiday representation is understandable and approved without regressing matching.
