## New Year Countdown

A full-screen landing page that counts down, live, to midnight on the next January 1st — days, hours, minutes, and seconds, ticking once a second over a background photo.

## Overview

On load, a short spinner (`img/spinner.gif`) shows for one second before it's removed and the countdown grid fades in in its place. Behind everything sits a background photo with a dark overlay, and the upcoming year is printed huge and translucent in the corner as a background flourish.

## Why I Built It

This one is about `Date` arithmetic and timing functions: computing a target date, taking the millisecond difference against "now," and turning that into four separate units without any date library. It's also a bit of CSS positioning practice — getting an overlay pseudo-element and a background number to line up correctly behind the real content.

## Features

- Countdown target is always "next January 1st," computed from the current year at load time (`new Date().getFullYear() + 1`)
- Days, hours, minutes, and seconds are recalculated and written to the DOM every second via `setInterval`
- All four units are zero-padded (`05` instead of `5`) so the layout doesn't jump around as digits change
- A one-second spinner delay before the countdown appears, via `setTimeout`
- The upcoming year is shown large and faint in the background

## JavaScript Concepts

- **`Date` arithmetic** — the countdown target is built with the explicit `new Date(year, monthIndex, day, ...)` constructor form (month is zero-indexed, so January is `0`) rather than parsing a date string, since only ISO 8601 strings are guaranteed by spec to parse consistently
- **Millisecond math** — the time remaining is a plain number of milliseconds, broken into days/hours/minutes/seconds with `Math.floor` and `%`
- **`setInterval` vs `setTimeout`** — `setInterval` drives the once-a-second tick for the whole countdown, while a single `setTimeout` handles the one-off spinner delay
- **Consistent formatting** — every unit uses the same `value < 10 ? '0' + value : value` pattern so none of the four boxes looks different from the others

## How It Works

`updateCountdown` takes the difference between the fixed target `Date` and `new Date()` right now, in milliseconds, then derives each unit by dividing down through days → hours → minutes → seconds and padding each with a leading zero when needed. The dark overlay is a `body::after` pseudo-element that's absolutely positioned; `body` itself is set to `position: relative` so that overlay's `100%` width and height are measured against the body's own box rather than whatever ancestor happens to be positioned.

## How to Run

Open `index.html` in a browser. The background image is loaded from Unsplash, so an internet connection is needed to see it.

## Technologies

HTML, CSS (including a `body::after` overlay and a responsive breakpoint), vanilla JavaScript `Date`/`setInterval`/`setTimeout`.

## What I Learned

Doing date math by hand instead of reaching for a library, and how a missing `position: relative` on a parent can make an absolutely-positioned child line up "by accident" until the page layout changes.

## Future Improvements

- Let the target date be configurable instead of hardcoded to "next Jan 1"
- Announce countdown changes to screen readers (currently there's no `aria-live` region on the numbers)
- Host the background image locally instead of hotlinking Unsplash
