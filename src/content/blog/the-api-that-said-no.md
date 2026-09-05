---
title: 'The API That Said No'
description: "When a vendor API won't do what you need, the work climbs a ladder: official endpoints, borrowed session tokens, then driving the browser by hand. There's a point where you should stop climbing."
pubDate: 'Sep 05 2026'
---

GHL's public API doesn't support the thing we needed. We knew this because we tried, got a clear error, and then went and read the docs. "Not supported yet." Not a bug, not a misconfiguration -- simply absent.

The feature existed in the web interface. We could see it working, right there, in the browser. The platform just hadn't exposed it via API.

So we started digging.

## Step one: watch the web app

When a vendor's API doesn't support something but their own UI does, the first move is to open the browser's network inspector and watch. Every request the web interface makes is a request you can potentially replicate.

The feature made a POST to an internal endpoint -- not in the docs, not versioned, exactly the sort of thing you shouldn't rely on. But there it was, working perfectly, used by the app itself to serve the feature the API refused to touch.

The authorisation header carried a bearer token: the user's session token, issued at login. We extracted it, put it in a script, and made the same POST. It worked.

For about four hours. Then it stopped. The session had expired.

## Step two: keep the session alive

A session token borrows the authority of an authenticated user. Before each call, we added a step: re-authenticate, get a fresh token, use that. More moving parts, but it held. The automation worked for weeks.

Until GHL updated their login flow and the authentication step started returning a different response structure.

## Step three: drive the browser

At this point we were three levels below the official API: undocumented endpoint, borrowed session authority, brittle authentication. The next rung was browser automation -- launch a headless Chromium instance, log in as the actual user, navigate to the feature, click the button programmatically.

This works. It is also, in some important sense, giving up. You are no longer interacting with the platform as a service. You are impersonating a human sitting at a desk. Every future UI change is a potential breakpoint. The platform can rearrange a button, rename a field, add a confirmation modal, and the automation breaks. Usually without notice and often without a clear error.

We got it working. It ran in production for a while. Then GHL added a CAPTCHA to the login page.

## The thing worth naming

There is a pattern here: the archaeology of unsupported features.

When a platform can do something but won't expose it via API, the gap gets filled by a workaround or a person. The workaround starts at the top -- try the unofficial endpoint, borrow the session, eventually drive the browser. Each step down is a trade: you stay automated, but you sacrifice stability and contract.

What I should have asked much earlier: at what level of fragility does the automation cost more than the manual step it replaces?

Browser automation is expensive to maintain. It breaks unpredictably. It requires a live browser in your stack. And the thing we were automating was -- when we actually looked -- a task that took a human about ninety seconds, happened twice a week, and could have been a button in a simple internal tool.

## The actual fix

The automation got replaced by an internal button that POSTs to the same undocumented endpoint. Still undocumented, yes. But now a human is in the loop, which means a human notices when it breaks. The maintenance cost dropped to nearly nothing. The failure mode became visible.

When the official API eventually supports this, we'll swap the internal tool for a proper API call. Until then: one button, one human, ninety seconds.

## How far down to climb

The ladder has a point where you should stop -- not because the next rung doesn't work, but because the further down you go, the further you are from a contract and the closer you are to an observation.

A contract is slow to break. When it does, it's versioned and usually announced. An observation -- a request format you inferred from watching the web app -- can change overnight. The platform doesn't owe you notice. You were never meant to be there.

The right question when you're two rungs below the official API: are you automating this because it's genuinely worth automating, or because you started climbing and stopping now feels like giving up?

Sometimes the honest answer is: it's a ninety-second button. Give the human the button.
