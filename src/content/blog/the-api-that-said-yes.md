---
title: 'The API That Said Yes'
description: 'GHL returned HTTP 200 and silently discarded every custom field update we sent. The request succeeded. The data never changed. We found out weeks later.'
pubDate: 'Apr 26 2026'
---

GHL returned HTTP 200. The request succeeded. The data never changed.

We only found out when a feature went live and none of the custom fields it relied on had any values. The writes had been silently swallowed for weeks.

## What happened

The Maverick Unavailability feature tracks which crew members are unavailable on which dates. The data lives in GHL as a custom field on each contact record. To write to it, you call `PUT /contacts/{id}` with a `customFields` array -- each entry containing a field ID and a value.

The bug: we had a placeholder field ID in the code. Something like `REPLACE_WITH_GHL_FIELD_ID`. A real-looking UUID was needed; we hadn't swapped it in yet for local testing. When we made the API call with the placeholder, GHL returned 200. The contact update was confirmed as successful. The field was silently ignored.

Not a validation error. Not a 404. Not a 422. HTTP 200. "OK."

When you look at the GHL response body after a successful contact update, it returns the updated contact record. The `customFields` array in that record reflects what GHL actually stored -- and if you checked it, the field wasn't there. But we weren't checking the response body on every write. You don't usually have to. An HTTP 200 is supposed to mean it worked.

## The specific failure mode

There's a class of API behaviour worth naming: the silent discard. The request was well-formed enough that the server accepted it. But the server applied only part of it, dropped the rest, and returned success anyway.

The GHL docs don't document this behaviour explicitly. You'd discover it by reading the full response body carefully, or by checking the stored data. Both require knowing the failure mode exists to think to look.

This is different from a malformed request. A malformed request gets a 4xx. You know immediately. The feedback loop is tight. With a silent discard, the feedback loop is as long as it takes to notice that the data didn't change -- which could be instantly, or could be when a user hits a feature that depended on it.

The 200 response codes you trust the most are the ones that can hurt you the most.

## Why it's hard to defend against

You cannot write a test that catches a silent discard unless you test what was actually stored, not just what the API responded with. This sounds obvious in retrospect. It's easy to miss in practice.

A typical write-path test might look like: call the function, assert no error was thrown, assert the API response status was 200. That test passes. The data was never written.

The correct test: call the function, then read the data back from the source of truth and assert it matches what you sent. That test would catch it. It also requires two round trips instead of one, and it requires knowing that the API might lie about success.

Most code I write doesn't assume the underlying API will lie. That's a reasonable default -- most APIs don't. But some do, silently, in specific conditions, without documentation. The only way to know is to test through the API, not up to it.

## What we changed

Two things.

First, the write function now reads back the updated field after every write and verifies it matches what was sent. If it doesn't, it throws. The test catches the class of error, not just the specific instance.

Second, we tightened the validation on field IDs. A field ID that doesn't match a known GHL field format now fails loudly before the API call is made. Placeholder strings don't make it to production.

The second fix is specific to this incident. The first fix is the general one.

## The broader point

HTTP 200 means the server processed your request. It doesn't mean your data is now stored. It doesn't mean what you sent was accepted in full. It means the server responded without error.

Most of the time, those are the same thing. Enough of the time that it's easy to conflate them. And then you get an API that says yes while doing nothing, and you spend time debugging in the wrong direction -- checking your code, checking your logic, not thinking to check whether the thing you wrote is actually there.

The lesson isn't "don't trust APIs." The lesson is: for data that matters, verify the outcome, not just the response. Read it back. Assert it. Don't let an HTTP 200 be the last word.

Especially if the API has a pattern of accepting unknown field IDs without complaint. That's a hint about the contract you're actually dealing with.
