# KAFUI-409 — Calendar Booking PASS, Rate Plan Blocks Wizard

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-409
- **Batch:** 01 — Hospitality Reservations (9–10 November)
- **Assignee:** Amer Hashima
- **Current Jira status:** **To Do — reopened 17 Nov 2026**
- **Original calendar requirement:** **QA PASS**
- **New linked end-to-end blocker:** **QA FAIL**
- **Previous QA reopen:** 11 Nov 2026 (Calendar range selection missing)
- **Current reopen:** 17 Nov 2026 (Reservation Wizard Step 2 blocker)
- **Jira QA comment:** 10825

## شرح التذكرة بالعربي

إحنا كنا طالبين نغيّر طريقة الحجز من الـTape Chart: نختار الغرفة، ونحدد الليالي المتتالية المتاحة من نفس الشارت، وبعدين نضغط **Book Selected Nights** علشان الـReservation Wizard يفتح بالـUnit والمدة المختارين.

**الجزء ده اتصلح فعلًا ونجح في اختبارنا.** الليالي المحجوزة مقفولة، وفي الحالات اللي الـCheck-Out فيها بدري ومسموح بعدها بحجز جديد، السيستم بيتعامل مع اليوم بشكل سليم. ونقدر نحدد عدد الليالي ونبدأ الحجز.

**لكن لقينا Blocker جديد بعد فتح الـWizard:** في Step 2 — Reservation Unit، الـUnit Type متحدد من الغرفة، إنما أي **Rate Plan** بنختارها بيظهر عليها تحذير إنه مفيش Rate متاحة، حتى لما جربنا Rate Plan جديدة من غير قيود أيام/مدة إقامة/حجز مسبق. بالتالي زر **Next** بيرفض الانتقال لآخر Step — Guest & Billing Information، والحجز نفسه ما بيكملش.

**مهم:** إعادة فتح التذكرة هنا بناءً على طلب الـQA بسبب الـCalendar → Booking end-to-end blocker. **ما نقولش إن اختيار الليالي فشل**: هو ناجح؛ المشكلة الجديدة في Rate Availability/Pricing في الـWizard.

## Original report

**Original description:** Calendar previously opened the wizard immediately on one selected room-day, with no pre-booking selection of consecutive room nights.

**Expected original behavior:** Choose a physical room, select consecutive available nights on Tape Chart, show interval/night count, use explicit Book action, open the wizard with selected unit/dates prefilled.

**Original real behavior:** Single-day click immediately opened Wizard; date range had to be edited there.

**Developer response:** Amer claimed Calendar range selection was implemented and Angular tests passed 135/135, with no live/browser E2E verification.

**2026-11-17 QA confirmation of original scope:** Room/night selection and Book Selected Nights now work in the deployed tester UI.

## Test Data / reproduction from QA

- **Entry point:** Hospitality → Reservations → Calendar / Tape Chart.
- **Step 1:** Select a room and available consecutive nights; click **Book Selected Nights**.
- **Step 2:** Reservation Wizard → Basic Information: passes.
- **Step 3:** Reservation Wizard → **Reservation Unit**: Unit Type already selected based on the room.
- **Step 4:** Try different Rate Plans, including a newly created plan with no stay-length or advance-booking restrictions.
- **Step 5:** Click **Next** to proceed to **Guest & Billing Information**.
- **Record IDs:** Exact Unit, Rate Plan IDs and dates not supplied; no values fabricated.

## Expected behavior

Calendar range selection works (confirmed). When a Rate Plan can be sold for that Unit Type and dates, the system should resolve a valid quoted amount and allow moving from **Reservation Unit** to **Guest & Billing Information**. Where a specific rate/pricing prerequisite is missing, the UI should identify it clearly.

## Actual behavior — exact messages

Yellow warning for every tried Rate Plan:

> This rate plan cannot be sold for the selected unit type and dates:
>
> - No rate is available for the selected dates and criteria.

On **Next**:

> Choose a rate plan that can be sold for these dates before continuing.

Even an unrestricted newly created Rate Plan produced this warning. The booking cannot advance from Step 2.

**Quoted Amount context (NOT a bug reported here):**

> Calculated by the system from the rate plan for the selected dates and guests. It cannot be edited.

The amount is now system-calculated/read-only. QA only notes this change; **do not treat read-only pricing itself as a defect**.

## Execution record — 17 Nov 2026

| Operation | Expected | Actual observed | QA |
| --- | --- | --- | --- |
| Choose available room and contiguous nights on Tape Chart | Range selection works | Works | **PASS** |
| Occupied-night selection / same-day checkout availability | Unavailable nights blocked; appropriate turnover allowed | QA confirms working | **PASS** |
| Click Book Selected Nights | Opens reservation wizard with selected stay | Works | **PASS** |
| Choose eligible Rate Plan in Reservation Unit | Valid rate resolved; Next to Guest & Billing succeeds | All tested plans show “No rate is available…” and Next blocked | **FAIL** |

## Action needed (no extra cases)

1. Identify why rate availability/pricing cannot resolve a sellable rate for the selected Unit Type, nights and Rate Plan.
2. Investigate the new Rate Plan without stay/advance restrictions as well; explain whether another configuration dependency is legitimately required.
3. Fix Step 2 so an actually sellable plan lets the tester advance to Step 3 and finish the reservation.
4. Tell QA what changed and where it was deployed; **preserve the now-working Calendar selection**.

## التاريخ وإعادة الفتح

| Date | Event |
| --- | --- |
| 10 Nov | Original Calendar multi-night UX change reported |
| 11 Nov | QA found original UI missing; ticket reopened |
| 11 Nov | Developer marked Done with Angular test evidence |
| 17 Nov | QA confirms Calendar selection works, but discovers rate-plan blocker in following Wizard |
| 17 Nov | Ticket reopened to To Do at QA's request, linked blocker documented |

## سؤال الـMeeting

**التعديل بتاع اختيار الليالي اتعمل، لكن ليه الحجز اللي بيبدأ من الـCalendar مش عارف يعدّي Step 2 بسبب “No rate is available” مع كل Rate Plan حتى الجديدة اللي بدون قيود؟ إيه الـRate Configuration الناقصة أو الـCode Fix المطلوب؟ وهل حد جرّب الفلو ده من المتصفح لحد آخر Step؟**

## Follow-up

- **Jira:** Reopened 17 Nov, To Do, assigned to Amer Hashima; QA comment 10825.
- **Acceptance:** Calendar scope remains QA PASS; linked reservation wizard issue passes only when a valid selected rate lets booking proceed.
