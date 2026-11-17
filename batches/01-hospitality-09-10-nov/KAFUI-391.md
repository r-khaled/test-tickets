# KAFUI-391 — Guest Name Without Profile + Name Search

- **Batch:** 01 — Hospitality / Reservations (reported 09–10 November 2026)
- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-391
- **Original title:** [Hospitality][Reservations] Capture Guest Name Without Profile and Allow Name Search and Filtering
- **Jira status at review:** Done — last status change 11 Nov 2026, 21:10 (Amer Hashima)
- **QA status:** **FAILED — PENDING REOPEN** (NOT ACCEPTED)
- **QA observation recorded:** 17 Nov 2026 (report from tester in the review conversation)
- **Actual Jira reopen:** **NOT YET PERFORMED** — do not confuse with this review file
- **Owner of QA verdict:** tester; developer implementation claims are not acceptance evidence

## شرح المشكلة بالعربي

لو حد اتصل يحجز لأول مرة، موظف الحجوزات ممكن يعمل Reservation من غير ما ينشئ له Guest Profile كامل. لكن Step 3 ما فيهاش حقل مستقل يكتب فيه اسم الشخص اللي حجز. بالتالي لما ييجي يدور على الحجز بعد كده، ما يقدرش يعتمد على اسم العميل، ويضطر يعرف رقم الحجز. المطلوب **إضافة اسم ضيف/جهة اتصال مستقل، حفظه، إظهاره، وإمكانية البحث والفلترة بيه**، من غير إجبار الموظف على Guest Profile.

**اللي حصل في إعادة الاختبار الحالية:** المختبِر رجع للشاشة في 17 نوفمبر وأكد إن **حقل إضافة اسم الضيف ما زال غير موجود**. إذن المتطلب الأساسي لم يُنفَّذ في البيئة المختبَرة. البحث بالاسم لم تتم إعادة تجربته في مراجعة 17 نوفمبر؛ لكن كان مسجلاً كفشل في QA Retest يوم 11 نوفمبر.

## Original report — scope (from Jira)

**Description:** Reservation Wizard Step 3 (Guest & Billing Information) has optional Primary Guest Profile, Customer Company, Company Payer and Booking Agent. When none is selected, there is no separate free-text lead guest/contact name. A profile-less booking can be found by Reservation Number but not by new guest name.

**Original test data:** An actual reservation was created without Primary Guest, Customer Company, Company Payer or Booking Agent. It was retrievable by Reservation Number. Exact reservation number and the test guest name were not documented; **do not fabricate them**.

**Original expected behavior (some business details need confirmation):**
1. Allow lightweight lead guest/contact name in Step 3 without mandatory Guest Profile.
2. Persist the name and display it in reservation detail/list/arrivals as approved.
3. Search and filter by guest/contact name alongside Reservation Number.
4. Preserve/reconcile that name if a full Guest Profile is linked later.
5. Confirm minimum contact details, name-matching, privacy/access policy with business owner.

**Original real behavior:** Profile-less reservation creation was possible; independent guest/contact name and name search/filter were not available.

**Action requested:** Confirm minimal data/search policy, implement create + persist + display + name search/filter end to end, ensure backward compatibility and safe later profile linking.

## Previous reopen / developer timeline

| Date | Event | Interpretation |
| --- | --- | --- |
| 10 Nov 2026 23:13 | Amer marked Jira **Done** | Developer closed issue |
| 11 Nov 2026 09:13 | Tester retested, filed **QA Retest — Not Accepted**, reopened to **To Do** | Missing independent name and name-based list search confirmed |
| 11 Nov 2026 21:10 | Amer moved **To Do → Done** again | **No later explanatory fix/comment in Jira** after the tester's failed retest |
| 17 Nov 2026 | Tester reports Step 3 still lacks independent guest name | **Current QA FAIL**, reopen required; no Jira status change performed yet |

**Developer evidence:** No new developer comment explaining how this issue was fixed after the QA reopen. Jira Done is not proof of a working feature.

## طريقة إعادة الاختبار — بالعربي

1. افتح **Hospitality → Reservations → Create Reservation** وامشِ في الـWizard لحد **Step 3 — Guest & Billing Information**.
2. سيب حقول **Primary Guest Profile / Customer Company / Company Payer / Booking Agent** بدون اختيار، واعرف هل فيه حقل **Guest/Contact Name** مستقل.
3. لو الحقل موجود، اكتب اسم اختبار وهمي واضح (مثلاً `QA Test Guest 391`) وسجّل الحجز ببيانات حجز صالحة.
4. افتح **Reservations List** واستخدم Search/Filter بالاسم اللي كتبته؛ لازم يظهر الحجز الصحيح من غير معرفة رقم الحجز.
5. افتح Reservation Details واعمل Refresh؛ الاسم لازم يفضل محفوظ.
6. جرّب اسم مختلف غير موجود؛ ما ينفعش يطلع الحجز بشكل خاطئ.
7. لو الـBusiness Rule اتحدد، اختبر ربط Guest Profile لاحقًا والتأكد إن الاسم/البيانات مش بتضيع.

**لو الحقل مش موجود من الخطوة 2:** سجّل Fail مباشرة؛ بقية السيناريوهات تصبح Blocked بسبب غياب الوظيفة الأساسية، مش Pass.

## Retest execution record

| Case | Input / action | Expected result | Latest observed result | Result |
| --- | --- | --- | --- | --- |
| 391-T01 | Step 3 without linked guest/profile | Independent guest/contact name input is available | 17 Nov: tester explicitly confirms **field absent** | **FAIL** |
| 391-T02 | Enter example contact name; save valid profile-less reservation | Name persists without mandatory profile | Cannot execute when independent name field absent | BLOCKED |
| 391-T03 | Search/filter Reservations List by saved name | Matching reservation appears | 11 Nov: unavailable; not freshly retested 17 Nov | PREVIOUS FAIL / RETEST PENDING |
| 391-T04 | Open detail/reload/list | Saved name displayed after reload | Blocked by missing field | BLOCKED |
| 391-T05 | Search for nonmatching name | No false-positive reservation | Not tested | PENDING |
| 391-T06 | Link full Guest Profile later (if approved) | Original booking data retained or explicitly reconciled | Business rule not confirmed | BUSINESS DECISION / PENDING |

**Execution environment/build:** Not provided; capture deployed URL/build before reproducing. **Test screenshots/video:** Not yet supplied. Do not invent test evidence.

## Decision / follow-up

- **QA verdict:** **FAIL / NOT ACCEPTED**.
- **Next Jira action:** Reopen KAFUI-391 **after tester approval to mutate Jira**; include step 2 failure and request the missing feature, not just a Done status update.
- **Developer must provide:** What exactly changed (UI, API, database, search/filter), where deployed, test evidence for end-to-end Create → Save → List Search → Detail, and any remaining business questions.
- **Acceptance gate:** QA confirms on the deployed UI that contact name capture, persistence, display and search/filter work end to end. Never accept purely from a unit-test number, agent message, or Jira status.

## Meeting questions

1. ليه التذكرة اتقفلت Done للمرة الثانية بعد ما الـQA أكد إن الحقل والبحث مش موجودين؟
2. إيه التغيير الفعلي اللي اتعمل بعد إعادة الفتح؟ فين الـcommit/deployment evidence؟
3. هل وظيفة الاسم المستقل متنفذة في الـUI والـAPI والـSearch فعلًا، ولا مجرد تعديل جزئي؟
4. لو فيه Business Decision مانع التنفيذ، ليه التذكرة اتقفلت Done بدل ما يتم توضيح الـBlocker؟

## Review history (append; never delete earlier outcomes)

- **2026-11-11 — QA FAIL:** No independent name or list search in UI; Jira issue reopened by tester.
- **2026-11-11 — Jira Done:** Developer closed again without a documented subsequent resolution comment.
- **2026-11-17 — QA FAIL / PENDING REOPEN:** Tester confirms independent guest-name input still absent. Further persistence/search cases cannot be accepted on this build.
