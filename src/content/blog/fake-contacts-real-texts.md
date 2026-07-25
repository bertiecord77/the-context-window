---
title: 'Fake Contacts, Real Texts'
description: 'Testing automation inside a live CRM means every fake contact is one trigger away from a real message. The conventions that keep rehearsal safe, and why they exist.'
pubDate: 'Jul 25 2026'
---

The GHL workflow fires on "contact created." It does not ask whether the contact is real.

Testing automation inside a live CRM requires creating contacts. Not contacts in a sandbox or a demo environment -- actual records in the same database the client's business runs on, in the same environment where a missed call triggers a WhatsApp message in under a minute. The moment a test contact appears in that database, every workflow configured to fire on new contacts sees it and considers whether to act. Most workflows have conditions. Often the blank test contact does not match.

Often.

## The near-miss

A test contact had been created to verify that a lead capture form was storing data correctly. It had a first name, a last name, a phone number, and no probe tag. The form verification took a few minutes. In those few minutes, a follow-up automation -- the kind that fires on any new contact without conditions, sends a WhatsApp, asks if it can help -- matched the record, queued the message, and was caught by a manual cancel before it dispatched.

The phone number in the test contact was a number we control. If the message had gone out, it would have gone to us. But that is not a rule you want to be working under. A test contact that nearly texts a number you own is structurally identical to a test contact that nearly texts someone else's.

## Three conventions

**Plus-addressed test emails.** Any test contact gets an email address in the form `probe+name@[a-domain-we-own].co.uk`. The plus addressing routes everything to a real inbox, so email delivery can be verified end to end. But the pattern is distinct, and any workflow that sends email can be given an exclusion condition: skip if the address contains `probe+`. That filter goes on every outbound email workflow, unconditionally.

**Probe tags at creation time.** Every test contact gets a tag -- "probe" -- before the record is saved. Not in a follow-up step. The tag is in the creation payload. No test contact ever exists in the database without it, even briefly. Workflows that send real messages are configured to exclude probe-tagged records.

This sounds obvious. It felt less obvious in the moment when the form verification was the only thing I was thinking about.

**Create and delete are separate scripts.** The instinct after testing is to tidy up: create the contact, run the verification, delete the contact, done, all in one script. The problem is that the delete step is the most likely to fail silently, and if it does, the contact remains indefinitely. Which would be fine -- it is tagged, it is excluded -- except that exclusion conditions get changed. Someone updates a workflow, removes the probe-tag exclusion because they forgot why it was there, and a set of old test records suddenly qualifies for live automation.

The cleaner pattern: the creation script creates and stops. A cleanup job runs on a schedule and deletes anything tagged "probe" that is older than 24 hours. If the cleanup job fails, nothing catches fire. The contacts are still tagged. The exclusion still holds. A failing cleanup job is a logged item, not a live incident. A failing deletion bundled into the probe script is a trap left in production.

## Rehearsal

Theatre companies rehearse on the same physical stage the audience will sit in front of. The props are real objects. The lights are real lights. What separates rehearsal from performance is not the environment -- it is the labelling. Props are marked as props. Everyone in the room knows which mode they are in.

The conventions do the same job. Probe tags, plus-addressed emails, separate cleanup jobs. They are not hygiene. They are the discipline that makes it safe to rehearse on a live stage while the client's business runs in the same database.

The near-miss made clear that relying on test contacts to not match live workflows was not that discipline. It was a hope. The conventions replaced the hope with a protocol.
