# KAFUI-391 — Guest Name Without Profile

- **Jira:** https://amerhashima.atlassian.net/browse/KAFUI-391
- **Batch:** 01 — Hospitality Reservations (9–10 November)
- **Assignee:** Amer Hashima
- **Current Jira status:** **To Do — reopened 17 Nov 2026**
- **QA result:** **FAIL — NOT ACCEPTED**
- **Previous QA failure / reopen:** 11 Nov 2026
- **Current failure / reopen:** 17 Nov 2026
- **Jira QA comment:** added on 17 Nov 2026 (comment 10823)

## شرح التذكرة بالعربي

إحنا بنحتاج نحجز لعميل جديد اتصل بالتليفون حتى لو لسه ما عندوش Guest Profile في النظام. المطلوب في Step 3 إن موظف الحجوزات يكتب اسم الضيف/الشخص المسؤول عن الحجز بشكل مستقل، ويحفظه، وبعد كده يقدر يدور على الحجز بالاسم من Reservations List. مجرد وجود Primary Guest selector لا يكفي لأنه بيحتاج Profile موجود بالفعل.

**نتيجة اختبارنا الحالية:** نفس المشكلة اللي رجعنا بسببها التذكرة يوم 11 نوفمبر لسه موجودة. دخلنا Step 3 يوم 17 نوفمبر، وما لقيناش حقل مستقل لإضافة اسم الضيف.

## Original report

**Description:** Reservations Wizard Step 3 supports optional Primary Guest Profile, Customer Company, Company Payer and Booking Agent but lacks an independent booking contact/guest name for reservations without a profile.

**Original test data:** Booking without Primary Guest, Customer Company, Company Payer, or Booking Agent. Booking could be retrieved by Reservation Number; exact number not recorded.

**Expected behavior:**
- Capture guest/contact name without creating a full Guest Profile.
- Save and display name with the reservation.
- Allow searching/filtering by the saved name in Reservations List.

**Real behavior (original and latest QA):** No independent Guest/Contact Name input. Previously the name search/filter was also absent.

**Action needed:** Implement the original Create → Save → Display/Search feature and show it working in the deployed browser UI.

## تاريخ التذكرة والمشكلة مع Done

| Date | Event |
| --- | --- |
| 10 Nov 2026 | Developer marked Done |
| 11 Nov 2026 | QA failed; issue reopened because name field and search were absent |
| 11 Nov 2026 | Developer marked Done again; no subsequent issue comment explaining the fix |
| 17 Nov 2026 | QA reconfirmed name field is absent; reopened again, To Do; assigned to Amer |

## الـRetest — المطلوب بالضبط

1. افتح **Hospitality → Reservations → Create Reservation → Step 3**.
2. من غير ما تختار Guest Profile أو شركة، شوف هل تقدر تدخل **Guest/Contact Name** مستقل.
3. بعد إصلاح الحقل، لازم الاسم يتحفظ ويظهر في الحجز وتقدر تبحث بيه في **Reservations List**.

## Execution record — 17 Nov 2026

| Step | Expected | Actual observed | QA |
| --- | --- | --- | --- |
| Enter Step 3 without linked Guest Profile | Independent Guest/Contact Name field exists | **Field is missing** | **FAIL** |
| Create/save reservation with name; search by name | Saved name visible and searchable | Not executed after missing field blocked the flow; previously failed 11 Nov | BLOCKED |

**Final QA verdict: FAIL; original requested feature still not delivered.**

## المطلوب من أمير قبل إنه يقفلها

- يوضح بالضبط إيه اللي اتغير في الحقل والحفظ والبحث، وإيه اللي نزل على بيئة الاختبار.
- يثبت إن الفلو شغال من الـWizard للـReservations List.
- ما يقفلش Done من غير تفاصيل التنفيذ ونتيجة اختبار في UI.

## سؤال الـMeeting

**ليه التذكرة اتقفلت Done تاني بعد فشل الـQA يوم 11 نوفمبر، وإحنا رجعنا يوم 17 نوفمبر ولقينا نفس الحقل لسه مش موجود؟**

## Follow-up

- **Jira action completed:** Reopened to To Do; Amer Hashima remains assignee; QA comment 10823 added.
- **Next acceptance:** QA tests actual field + save + name search after the fix.
