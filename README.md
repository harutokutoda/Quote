# random quote generator

A single HTML file that shows you a random quote. No build step, no npm install,
no framework. Just open the link.

I built this because I wanted something small to put on a "new tab" page and got
tired of the ones that either needed an API key or hit one endpoint that was down
half the time.

**Live here:** https://harutokutoda.github.io/Quote

## What it does

Click the button, get a quote. That's most of it.

Behind the scenes it tries a few free quote APIs in order until one answers. If
they're all down — which happens more than you'd think — it falls back to a list
of 20 quotes hardcoded in the file. So it always shows *something*.

There's a 4 second cooldown after each fetch so you can't hammer the APIs. The
cooldown bar is the thin line at the bottom of the button.

Copy button puts the quote on your clipboard as `"quote" — author`.

## Keyboard

- `Space` or `Enter` anywhere on the page → new quote
- `Tab` to the copy button and press `Enter` → copies (the global shortcut gets
  out of the way when you're focused on a button)

## APIs it uses

In order:

1. `quoteslate.vercel.app` — returns a bare object, no array wrapper
2. `dummyjson.com/quotes/random` — reliable, has IDs
3. `api.realinspire.live` — backup
4. `api.quotable.io` — last resort. This one has been flaky for a while, it's
   only here in case the others are down

Each one gets 2 attempts with a bit of backoff, and the whole chain has a 9
second budget. If it blows past that, it gives up and uses a fallback quote.

## Accessibility

I tried to not make this terrible:

- Live regions on the quote and the status line so screen readers announce
  updates
- The button uses `aria-disabled` instead of `disabled`, because a disabled
  element can't hold focus and it kept kicking keyboard users back to the top
- Skeleton loaders are `aria-hidden` — they were making the live region
  chatter
- `prefers-reduced-motion` kills all the animations, including the drifting
  gradient background

## Things I know are a bit off

- The drift animation on the background is a 300%-sized gradient and it runs
  forever. It's pretty but it's not cheap.
- The cooldown is fixed at 4 seconds. It's not configurable without editing
  the file.
- No history, no favorites, no "previous quote". I keep meaning to add it.
- The quote APIs occasionally return something that isn't really a quote
  (fragments, mis-attributions). Nothing I can do about that from here.

## License 

Do whatever you want with it.
