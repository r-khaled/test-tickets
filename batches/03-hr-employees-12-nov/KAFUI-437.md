# KAFUI-437 — Initial Work Schedule Missing from Employee Work Schedules Card

- **Original Jira:** https://amerhashima.atlassian.net/browse/KAFUI-437
- **Batch:** 03 — HR Employees (tickets reported 12 November 2026)
- **Parent:** KAFUI-411 — HR Module (Employees Bugs)
- **Issue type:** Jira Subtask
- **Assignee:** Amer Hashima
- **Jira Priority:** Medium
- **Current Jira status:** **To Do — reopened on 17 November 2026**
- **QA status:** **FAIL / NOT ACCEPTED**
- **Jira QA comment:** 10841
- **Original implementation claim:** Initial schedule chosen when creating an employee is automatically persisted as the first employee schedule assignment, starting on Joining Date; developer claimed integration coverage and noted a local code change needing deployment
- **Tester confirmation date:** 17 November 2026
- **Scope:** Exact originally reported failure only; do not append unrelated tests

## الشرح بالعربي — إيه المشكلة بالضبط؟

وقت ما بنعمل **Add New Employee** بنختار له **Work Schedule أصلي**، والجدول ده بيظهر بعد الحفظ في **Employee Information** (المثال اللي اتشاف حاليًا: `QA Full Time`).

المفروض ننزل تحت على **Employee Work Schedules Card** ونلاقي نفس الجدول ده موجود كأول Assignment؛ وعايزين **Label واضح إنه Original / Initial Work Schedule** علشان نفرّقه عن أي جداول عمل تتضاف للموظف لاحقًا.

**اللي لسه بيحصل:** جدول `QA Full Time` ظاهر في Employee Information، لكن **مش موجود كـRecord في الـCard**. ده نفس الـGap اللي التذكرة الأصلية اتعملت بسببه، رغم إن أمير كان قال إنه اتصلح.

**الحاجات اللي نجحت فعلًا ومش هنفتحها تاني:**
- نقدر نضيف أكتر من Work Schedule Assignment للموظف.
- السجلات الجديدة ما ينفعش تتداخل في الـEffective Date Range؛ الـOverlap Validation بقت شغالة.
- دول متعلقين بـ**KAFUI-439**، اللي QA أكد إنها بقت PASS.

**KAFUI-438** كانت عن تعارض معنى الجدول في الـHeader مع الـActive Assignment في الـCard؛ لا نعتبرها فاشلة مجددًا من غير اختبار مستقل للجدول الساري وتفسيره. المشكلة الحالية الموثقة هي **غياب أول Assignment**.

## Original Report — from Jira

### Description
The Work Schedule selected during Employee Create appears in Employee Information/header but not in the **Employee Work Schedules** card. The developer needs to clarify and/or synchronize initial assignment with assignment history.

### Test Data (original)
A QA Employee M3 had a Night Work Schedule chosen on Create. After saving, Employee Information showed Night, but the schedule card had no row. Employee identifiers and exact dates were not provided.

### Expected Behavior
If selecting Work Schedule during Employee Create means assigning that schedule to the employee, the first assignment should appear in the card/history, or the UI must explicitly explain why the header default and assignment history differ.

### Real Behavior (original)
The initial schedule was shown in the header but absent from the Work Schedules Card.

### Action Needed (original)
Check header versus schedule-history data source and fix initial assignment persistence/loading as required. Verify Save, refresh and history.

### Ticket Priority
Medium — initial-schedule visibility/history issue, no independent payroll failure demonstrated.

## Developer response — 16 November 2026

Amer claimed:
- The Work Schedule selected during Employee creation was now automatically added as the first Assignment in Employee Work Schedules Card.
- Assignment start date was set to the employee Joining Date.
- Integration tests passed.
- The implementation was a **local change requiring deployment**.

These are **implementation claims**, not independent proof of the current tested UI behavior.

## QA Retest — 17 November 2026

### Test Data
- Screen: HR → Employees → Employee Information and Employee Work Schedules Card.
- Original Work Schedule shown in employee info: **`QA Full Time`**.
- Exact employee number, initial Effective Date, and assignment record IDs: **not supplied**.

### Expected Behavior
The initially chosen Work Schedule should be visible as the first item in the Employee Work Schedules Card, distinguishable with a label such as **Original Work Schedule** or **Initial Assignment** and retaining its actual date. Additional subsequent assignments should coexist as permitted, without overlap.

### Real Behavior
`QA Full Time` appears in Employee Information, but the original Work Schedule is **not listed in the Employee Work Schedules Card**.

### Action Needed
1. Make sure that choosing a Work Schedule at Employee creation records an appropriate initial Assignment (where that's the agreed data model), or the card loads the already-persisted first Assignment.
2. Display that original schedule in the card with an explicit **Original / Initial** label and actual effective date.
3. Keep the now-working ability to add later assignments with non-overlapping validity.
4. Demonstrate in the deployed UI that the original assignment appears after Save/Refresh, and explain what was actually changed since the last Done.

## Execution Record

| Test | Expected | Actual tester observation | QA verdict |
| --- | --- | --- | --- |
| Original schedule appears in Employee Information | Displays initial schedule | `QA Full Time` visible | PASS |
| Original schedule appears in Work Schedules Card | First assignment listed and labelled as original | Initial schedule absent from Card | **FAIL** |
| Add another Work Schedule | Additional assignment permitted when valid | Works | PASS — KAFUI-439 |
| Prevent effective-date overlapping | Overlapping assignments rejected | Works | PASS — KAFUI-439 |

No new identifier, date or screenshot was supplied; do not invent them.

## History / Follow-up

| Date | Event |
| --- | --- |
| 12 Nov 2026 | Original report filed: initial schedule missing from card |
| 16 Nov 2026 | Developer marked Done and claimed first assignment now auto-created, integration-tested; needs deployment |
| 17 Nov 2026 | User retested: original schedule still absent from Card, while multiple schedule assignments/non-overlap now work |
| 17 Nov 2026 | QA comment 10841 added; issue reopened to To Do; remains assigned to Amer Hashima |

## Meeting question

**إزاي KAFUI-437 اتقفلت Done مع تصريح إن الـInitial Assignment بتتسجل تلقائيًا، لكن إحنا في الـUI لسه بنشوف Original Work Schedule في Employee Information ومش بنلاقيها خالص في الـCard؟ فين التغيير اللي اتنشر، وهل في فرق بين Header Default وAssignment History؟**

## Acceptance Gate

Tester confirms that the existing original schedule is visible as the **first/initial assignment** with a clear label on the Employee Work Schedules Card after reload, and adding subsequent valid non-overlapping assignments continues to work. A passing integration test or Jira Done alone is not QA acceptance.
