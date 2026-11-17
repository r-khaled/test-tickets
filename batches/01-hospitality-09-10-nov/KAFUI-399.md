# KAFUI-399 — Hold Works but Cannot Be Released

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-399
- **Batch:** 01 — Hospitality Reservations (9–10 November)
- **Assignee:** Amer Hashima
- **Current Jira status:** **To Do — reopened 17 Nov 2026**
- **QA result:** **PARTIAL FIX / FAIL on Hold release**
- **Previous QA reopen:** 11 Nov 2026
- **Current follow-up reopen:** 17 Nov 2026
- **Jira QA comment:** 10824

## شرح التذكرة بالعربي

كان عندنا مشكلة إن الـReservation ما كانش بيقبل **Place on Hold** حتى لو تاريخ الـExpiration صحيح. في الاختبار الجديد اتأكدنا إن **عمل الـHold نفسه بقى شغال**، ودي نقطة اتحلت.

لكن المشكلة دلوقتي إن بعد ما الحجز يتحول إلى **HOLD**، لما ندخل نعمل Edit نلاقي **Status History** بس، ومفيش طريقة متاحة تغيّر الـStatus أو تشيل الـHold. فالحجز بيفضل عالق في Hold، رغم إن المفروض تكون فيه طريقة واضحة نقدر نعمل بيها Release/Unhold وفق قواعد النظام.

**ما نفتحش موضوع تاريخ انتهاء Hold أو سيناريوهات إضافية دلوقتي. الاختبار هنا محدد: هل أقدر أشيل الـHold بعد ما عملته، ولا لأ؟**

## Original report

**Description:** Placing a reservation on Hold previously failed with a conflict despite choosing a valid future Hold Expiration before Check-In.

**Original test data:** Future stay in late November; Hold Expiration 15 Nov 2026, tested 10 Nov. Exact reservation ID and check-in not recorded consistently.

**Original expected behavior:** Valid Place on Hold succeeds and stores HOLD status/expiration.

**Original real behavior:** Conflict error prevented placing the hold.

**Developer response:** Amer reported successful API/SQL hold lifecycle checks and UI error-handling tests, but no verified live UI completion at the time.

## Retest scope and actual finding — 17 Nov 2026

**Test data:** Reservation where **Place on Hold now succeeds**. Exact Reservation Number not supplied.

**Expected behavior:** The user can place an eligible Reservation on Hold, then access a supported action to remove/release that Hold or otherwise move to an allowed next status; the state must update successfully.

**Actual behavior:** Hold is created; **status = HOLD**. On **Reservation Edit**, the user sees **Status History** but cannot change the status or remove the Hold. Trying to get out of Hold leaves the reservation stuck.

**Action needed:** Make the valid Hold-release/status-transition action available and functional from the applicable Reservation screen, respecting approved state-transition rules. Record what was changed and where deployed.

## خطوات إعادة الاختبار (الحالة اللي اتبلغت فقط)

1. افتح Reservation مناسبة واعمل **Place on Hold** — العملية أصبحت تنجح.
2. افتح **Edit / Reservation Details** بعد ما الحالة تبقى **HOLD**.
3. حاول تستخدم أي إجراء متاح علشان تشيل الـHold أو تغيّر الحالة بشكل صحيح.
4. **النتيجة المرصودة:** المعروض Status History فقط، ومفيش طريقة فعالة للتغيير، والحجز بيفضل **HOLD**.

## Execution record

| Step | Expected | Actual observed | QA |
| --- | --- | --- | --- |
| Place on Hold | Status becomes HOLD | Works successfully | **PASS** |
| Edit reservation / remove or release Hold | Supported transition succeeds; status changes | No usable transition; status stuck at HOLD | **FAIL** |

**Final QA verdict: PARTIAL FIX, NOT ACCEPTED due to Hold release failure.**

## التاريخ وإعادة الفتح

| Date | Event |
| --- | --- |
| 10 Nov | Initial hold conflict issue reported |
| 11 Nov | Previous QA follow-up/reopen because real UI Hold flow lacked acceptance |
| 11 Nov | Developer returned issue to Done after reporting automated tests, not live browser verification |
| 17 Nov | QA confirms creating Hold now passes but cannot remove it; issue reopened to To Do |

## المطلوب من أمير قبل Done

- يوضح الـUI action الفعلي للـRelease/Unhold، وقاعدة انتقال الحالة المسموح بها.
- يصلح مشكلة إن حالة الحجز بتفضل HOLD ومش بتتغير.
- يكتب إيه اللي اتصلح وإيه اللي اتنشر، مع إثبات عملي للفلو من الـUI.

## سؤال الـMeeting

**إزاي عملية Place on Hold اتصلحت لكن مفيش طريقة نخرج بيها الحجز من حالة Hold؟ فين تنفيذ الـStatus Transition من الـUI، وهل اتاختبر أصلًا؟**

## Follow-up

- **Jira:** Reopened 17 Nov, To Do, assigned to Amer Hashima; QA comment 10824.
- **Next acceptance:** Tester successfully removes/releases Hold via supported UI path, and confirms updated status.
