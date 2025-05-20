---
date: '2025-05-20T11:10:23+05:30'
draft: false
title: 'Web Accessibility Wcag'
---

## What is WCAG 

Web Content Accessibility Guidelines (WCAG) are a set of rules created to make the web more accessible for users. Various versions of these guidelines have been published, with the latest being WCAG 2.2. WCAG 3.0 has also been proposed, but it is currently an incomplete draft. Therefore, we will focus on the conformance requirements for WCAG 2.

## Principles based on which these guidelines are made

The principles — also known by the acronym POUR — are Perceivable, Operable, Understandable, and Robust.

### Perceivable

Content must be presented in ways that users can perceive

### Operable

UI components and navigation must be operable by all users 

### Understandable

The content and interface must be understandable to everyone

### Robust

The application must be built so that assistive technologies can access it; it must be compatible with as many user agents as possible.

### Levels A, AA and AAA

Based on these principles and varying levels of accessibility needs, WCAG defines three levels of conformance—A, AA, and AAA. Below are the requirements for each level:

#### Level A
----
- Provide alt text for images.
- Include transcripts/captions and audio descriptions for prerecorded media.
- Use ARIA attributes and roles along with semantic HTML.
- Any audio longer than 3 seconds must have a pause, stop, and volume control.
- Ensure keyboard navigation for all functionality except tasks like drawing or whiteboarding.
- Do not impose time limits on app usage; if you do, warn users and allow an easy way to extend.
- Provide the ability to pause, stop, or hide moving, scrolling, or blinking content.
- For flashing content, keep flashes to three per second or below the [general threshold](https://www.w3.org/TR/WCAG20/#general-thresholddef)
- Ensure page titles, focus order, and links have self‑descriptive text.
- Declare the language of each page programmatically (e.g., <html lang="en">).
- If user input errors occur, present error messages in text.
- Avoid duplicate attributes or IDs; ensure each element’s name, role, and value can be accessed by assistive technologies.

#### Level AA
----
- Everything in Level A
- Provide captions and audio descriptions for live media.
- Maintain a color contrast ratio of at least 4.5:1 (except for logos).
- Allow text to be resized up to 200% without loss of content or functionality.
- Ensure mandatory headings and labels are present
- Keyboard focus must be visible.
- For user input errors where suggestions for correction are known, provide those suggestions.

#### Level AAA
----
- Everything in Level AA
- Provide sign language interpretation and audio descriptions for prerecorded media.
- Provide audio descriptions for live audio.
- Maintain a color contrast ratio of at least 7:1 (except for large text or logos).
- Do not include background audio, or keep it below 20 dB.
- Limit text line width to 80 characters or fewer.
- Do not justify text.
- Use at least 1.5× line‑height spacing between paragraphs.
- Ensure full keyboard navigation with no exceptions.
- Do not impose any time limits on app visits.
- For flashing content, keep it below three flashes per second.
- Provide users with location information (e.g., breadcrumbs, landmarks).
- Include section headings to organize content.
- Offer a way to identify unusual words and expand abbreviations.
- Provide pronunciation information for words.
- Include clear help text for each action.