# KAFUI-613 — Work Schedule Save Gives No Validation Feedback

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-613
- **Area:** HR → Setup → Work Schedules → Add New
- **Origin:** QA follow-up to Batch 02 (HR Setup, originally reported 11 Nov 2026)
- **Created:** 17 Nov 2026
- **Parent:** [KAFUI-410 — HR Module (Setup Bugs)](https://amerhashima.atlassian.net/browse/KAFUI-410)
- **Relates to:** [KAFUI-414 — Unsaved Create/Edit form warning](https://amerhashima.atlassian.net/browse/KAFUI-414)
- **Assigned:** Shaimaa Kamal
- **Jira priority:** Medium
- **Ticket type:** Bug (Jira Subtask)
- **QA status:** NEW REPORTED DEFECT — pending developer fix / later UI retest

## شرح المشكلة بالعربي

شاشة **Work Schedules → Add New** اتغير تصميمها. لما بنضغط **Save** ولسه فيه حقول مطلوبة فاضية أو قيم مش مستوفية شروطها، العملية ما بتكملش، **لكن مفيش رسالة Validation** تقول لنا الحقل الغلط أو الناقص. المستخدم يفضل يدور بنفسه على المشكلة من غير أي إرشاد.

دي مش نفس مشكلة **KAFUI-414**: هناك المطلوب تحذير قبل الخروج من الفورم وفيه تغييرات ما اتحفظتش؛ هنا المطلوب **توضيح الخطأ عند Save**. ولذلك اتعملت تذكرة منفصلة وارتبطت بـ414 على Jira بعلاقة Relates.

## Original QA report (2026-11-17)

**Description:** Missing actionable required-field/input validation messages during Save in the redesigned Work Schedule creation form.

**Test Data:** HR > Setup > Work Schedules > Add New; leave required inputs missing or enter a value judged invalid and click Save. Exact input names, values and IDs were not supplied. Do not invent them.

**Expected Behavior:** Save-blocking required-field or invalid input errors are shown clearly next to the affected inputs (or in an equivalent understandable visible summary). User can correct the values and submit the valid form. If the button is disabled, the form should still communicate unmet requirements rather than be silent.

**Real Behavior:** User clicks Save; nothing useful happens and no visible validation message explains the missing/invalid input.

**Action Needed:** Fix/restore required-field and Save validation feedback in redesigned Work Schedules Create. Test incomplete data → Save → error shown → correction → successful save in deployed UI; record actual change and deployment evidence.

**Ticket Priority:** Medium (non-blocking validation/UX defect; no proven financial/data loss or entire module outage).

## خطوات الاختبار الفعلية — بدون سيناريوهات زيادة

1. افتح **HR → Setup → Work Schedules → Add New**.
2. سيب Required Field من غير قيمة (أو اكتب قيمة غير مقبولة معروفة من نفس الشاشة).
3. اضغط **Save**.
4. **Expected:** رسالة واضحة تحدد الحقل المطلوب/الغلط وطريقة تصحيحه.
5. **Observed 17 Nov:** لا توجد رسالة خطأ مفيدة، والحفظ لا يكتمل.

## Execution record

| Date | Test | Actual result | Verdict |
| --- | --- | --- | --- |
| 17 Nov 2026 | Save incomplete/invalid redesigned Work Schedule form | No visible input-level validation; Save does not complete | **FAIL — NEW BUG** |
| Future retest | Repeat the same case after announced fix | Not yet performed | PENDING |

## Meeting question

**ليه الـWork Schedule الجديد لما Save يفشل بسبب Required Fields أو Input Validation ما بيطلعش أي رسالة تعرف المستخدم يعمل إيه؟ مين راجع الـUI بعد إعادة التصميم؟**

## Follow-up

- **Jira:** Created as KAFUI-613 under existing KAFUI-410, Medium, Shaimaa Kamal.
- **Relationship:** Relates to KAFUI-414 (same redesigned Work Schedule form; separate functionality).
- **Acceptance:** Visible validation feedback on failed Save attempt, followed by successful Save after correction.
