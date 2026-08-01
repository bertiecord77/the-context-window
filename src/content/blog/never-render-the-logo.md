---
title: 'Never Render the Logo'
description: 'Image models produce a plausible wordmark every time. Plausible is the worst possible outcome. The fix is a procedure, not better prompting.'
pubDate: 'Aug 01 2026'
---

The prompt was specific: hero image, client's logo top left. The image came back in seconds. Good composition, confident typography, the logo looking almost exactly right -- same colour family, same general letterform, the right number of characters in the right arrangement.

Almost exactly right is the specific problem.

## What image models do with a brand mark

When you ask an image model to include a logo, it does not look up the real file. It constructs something from whatever it has seen: the wordmark as it appeared in training data, across all its contexts, averaged and compressed into a plausible rendering. If the brand is well-represented in that data, the result might be close. If it is not, the result will be a confident guess.

Either way, what you receive is a confabulation. The model does not know it is confabulating. The image looks authoritative.

The reason this is hard to catch is that the error is not obvious at first look. A hallucinated fact reads wrong if you know the subject. A code suggestion that doesn't compile fails loudly. A near-but-wrong wordmark looks fine in the preview, fine in the Slack message, possibly fine in the PDF. It looks wrong the moment someone who knows the brand -- or the client whose brand it is -- compares it to the real file.

## Why near-but-wrong is the worst outcome

Brand marks are not approximate. A stock image can be roughly appropriate. An illustration can be an approximation. Body copy can be revised. A wordmark is fixed: the specific curve of each letterform, the exact tracking between characters, the precise stroke weight, the colour values to four decimal places in HEX and CMYK both. These were decided once and locked. They are correct by definition.

"Near" means wrong. There is no partial credit for brand marks.

A wrong logo in a published asset is not a minor error. It is a reproduced mistake at whatever scale the asset reaches. Correcting it means finding every instance and replacing it. If the asset is a website hero, that means a new deploy. If it is a printed item, the print run is wasted. The cost scales with the distribution.

## The fix

Never ask an image model to render a client's logo.

Instead: generate the image with a deliberate placeholder where the logo belongs. A solid-colour rectangle, correctly sized and positioned, representing where the real file will go. The generated image contains every element except the one element that cannot be approximated.

Then composite the real SVG or PNG in afterwards. Drop it onto the placeholder. Align it, size it to match the space. The final file contains the actual brand asset, at its correct resolution and colour profile, not a generated approximation of it.

This adds one step. That step is straightforward and takes about thirty seconds in any image editor.

The alternative is checking every generated image against the brand guidelines to verify the rendered logo is correct. That requires having the guidelines, knowing the mark well enough to spot deviations, and doing that check every time, including when the preview looks good and the deadline is close. That is a lot of conditions to satisfy reliably.

Procedures beat disciplines. A step that is built into the workflow cannot be skipped by accident. Attention can drift; a defined step does not.

## When this generalises

Brand marks are a specific case of a broader pattern: when an error is hard to detect but expensive to correct, you cannot rely on catching it during review. The gap between "looks fine" and "is wrong" is exactly where errors live permanently.

The class of errors worth worrying about is not the visible ones. Visible errors get caught. The ones that survive into production are the ones that look like they're right.

The rule of thumb is: for anything where the cost of a mistake scales with distribution, and where the mistake is not obviously visible, take the action that makes the mistake impossible rather than the action that makes it unlikely. A procedure that prevents a wrong logo from being generated is better than a review step that might catch it. The review might miss it. The procedure cannot.

There is no version of an image generation workflow where "check the logo carefully" is more reliable than "never ask it to generate the logo." One depends on vigilance. The other does not.

Composite the real file. Every time.
