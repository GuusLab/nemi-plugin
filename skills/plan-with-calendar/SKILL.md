---
name: plan-with-calendar
description: "Read and plan the user's Nemi calendars and set up Nemi Meet video calls. Use when the user asks what is on their agenda, when they are free, to schedule, move, repeat or cancel something, to plan a week, or to create a meeting link."
---

# Calendars and Meet

## The big caveat

Only calendars **made in Nemi** are reachable. Calendars the user connected from Google, Outlook or an ICS feed cannot be read or written through this connection, so an agenda read here may be missing things. When the user asks about their whole schedule or whether they are free, answer from what you can see and say once, briefly, that connected calendars are not included.

## Reading

`calendar_events` with `start` and `end` as ISO 8601 instants (default: the coming seven days; at most a year). Repeating events come back expanded. The result includes the calendars too, which is how you get a `calendar_id`.

- Resolve "today", "next week", "Thursday" against the current date in the user's time zone. If you do not know the zone and it matters, ask once or use the one the events carry.
- Present an agenda grouped by day, in time order, with times in the user's zone and 24-hour or 12-hour format to match how they write.
- For "when am I free", list the gaps inside working hours (09:00 to 17:00 unless the user says otherwise) and repeat the caveat about connected calendars.

## Creating events

`calendar_event_create` needs `calendar_id`, `title`, `start` and `end`.

- If the user has one calendar, use it. If several, pick the one whose name fits ("Work", "Family") or ask.
- Include the offset in times (`2026-10-02T14:00:00+02:00`) or set `timezone` (IANA, e.g. `Europe/Amsterdam`). Never send a bare local time without either.
- No end given: one hour for a meeting, 30 minutes for a call, all day for a birthday or holiday.
- All-day events: `all_day` true, `start` at the date, `end` at the next day (the end is exclusive).
- Repeats: `rrule` in RFC 5545 form without the `RRULE:` prefix: `FREQ=WEEKLY;BYDAY=MO`, `FREQ=MONTHLY;BYMONTHDAY=1`, `FREQ=WEEKLY;INTERVAL=2;BYDAY=TU,TH;COUNT=10`, `FREQ=YEARLY`.
- Add `location` and `description` when the user gave them.

## Changing and cancelling

- `calendar_event_update` changes only the fields you send. An event cannot move to another calendar. An empty `rrule` stops it repeating.
- Occurrences of a repeating event carry the series' own `event_id`, so `calendar_event_update` and `calendar_event_delete` on one of them change or remove the WHOLE series. Say so and confirm first. An occurrence that was edited on its own has an id of its own and a `recurring_event_id` pointing at its series. There is no tool to change or skip just one occurrence; to cancel a single date, tell the user to do it in the Nemi Calendar app.
- Find the `event_id` with `calendar_events` over the right window.

## Meetings with a video link

`meet_create` makes a Nemi Meet link. `scheduled_at` is only a label; it does not add anything to a calendar. To schedule a call properly:

1. `meet_create` with `title` and `scheduled_at`, and `lobby`, `password` or `guests_allowed` false when the user wants control over who gets in.
2. `calendar_event_create` for the same time with `meeting_url` set to the join link.
3. Reply with the time and the join link in full.

Every call holds at most ten people. Joining, recording and captions happen in a browser. `meet_list` shows existing meetings; `meet_delete` stops a link working immediately, so confirm first.

## Planning a week

When asked to plan or review a week: read the window, list fixed commitments, point out clashes and back-to-back runs without a break, then propose blocks for what the user wants to fit in. Create nothing until the user agrees to the plan, then create the agreed events in one go and list what was added.
