# Driving the portal with a browser tool

The filing UI is Angular with Angular Material. Every technique below exists
because the obvious approach fails silently — the click lands, nothing happens,
and no error appears anywhere.

## Reach elements through the DOM

Read and click through the DOM rather than screenshot coordinates. The page scroll
position shifts between taking a screenshot and issuing the click, so a click aimed
at one row reliably lands on another.

Where coordinates are unavoidable, compute them fresh and remember the two spaces
differ — screenshots come back at 800px wide against a ~966px viewport, so scale by
`800 / window.innerWidth`.

## A leaked backdrop swallows every click

The costliest failure, because it is completely silent. An Angular Material
dropdown can close and leave its `cdk-overlay-backdrop` behind, with no overlay pane
attached. The invisible backdrop then intercepts every click on the page: buttons
receive focus rings, coordinates are correct, and nothing responds.

Diagnose by asking what is actually on top:

```js
const b = [...document.querySelectorAll('button')].find(x => x.textContent.trim() === 'Delete');
const r = b.getBoundingClientRect();
document.elementFromPoint(r.left + r.width / 2, r.top + r.height / 2);
// -> DIV.cdk-overlay-backdrop  means the backdrop is swallowing the click
```

Clear it, and clear it after every `mat-select` interaction as routine:

```js
document.querySelectorAll('.cdk-overlay-backdrop').forEach(b => b.remove());
document.body.classList.remove('cdk-global-scrollblock');
```

## Hidden modals hold decoy buttons

The DOM carries several dormant modals — a session-timeout dialog, an external-link
disclaimer, confirmation dialogs. Their buttons match the same text selectors as the
real ones, so a search for "Confirm" or "Continue" can return a decoy inside a
`DIV.modal fade` that is `display: none`.

Filter to what is actually rendered:

```js
const real = [...document.querySelectorAll('button')]
  .filter(x => x.textContent.trim() === 'Confirm')
  .find(x => x.getBoundingClientRect().height > 0);
```

A zero-height element is a decoy. A button that exists, reports `disabled: false`,
and still has zero dimensions is inside a hidden modal.

## The render collapses after a long scroll

Scrolling far down a long schedule can leave the pane painting blank or partial
while the DOM stays intact — JS reads work, screenshots show nothing.

`window.scrollTo(0, 0)` restores it. Where a footer button is needed, collapsing
the page's sections first ("Collapse All") brings it into view without a long scroll.

## Clicking what Angular listens to

`.click()` in JS fires reliably on `<li>` rows and most controls, and fails on some
Angular submit buttons — Continue and Save among them, which need a real mouse
event.

The most robust navigation on the Return Summary is a JS click on the row's `<li>`,
which is immune to the render glitch:

```js
[...document.querySelectorAll('li')]
  .filter(e => /Schedule Capital Gains/.test(e.innerText || ''))
  .sort((a, b) => a.innerText.length - b.innerText.length)[0]
  .click();
```

Sorting by text length picks the innermost matching row rather than an ancestor
containing the whole list.

## mat-select needs click-then-click-option

Keyboard navigation on these dropdowns is unreliable. Open the control, then click
the option element, then clear the backdrop:

```js
sel.click();
[...document.querySelectorAll('mat-option')]
  .find(o => o.textContent.trim() === 'Others').click();
document.querySelectorAll('.cdk-overlay-backdrop').forEach(b => b.remove());
```

## Writing values Angular will keep

Setting `.value` directly leaves Angular's model unchanged, so the value renders
and is then discarded on save. Use the native setter and dispatch the events the
framework listens for:

```js
const setVal = (el, v) => {
  const proto = el.tagName === 'TEXTAREA'
    ? window.HTMLTextAreaElement.prototype
    : window.HTMLInputElement.prototype;
  Object.getOwnPropertyDescriptor(proto, 'value').set.call(el, String(v));
  ['input', 'change', 'blur'].forEach(t => el.dispatchEvent(new Event(t, { bubbles: true })));
};
```

This also sidesteps the trailing-zero bug — amount fields pre-filled with `0` turn
a typed `22930` into `229300`, because the old zero survives. Replacing the whole
value avoids it.

## Identify fields by label, not position

Field order shifts as `mat-select` elements interleave with `<input>` elements, so
an index taken from one enumeration misaddresses another. Writing a place name into
a date field clears the date and produces a mandatory-field defect two screens later.

Resolve by label:

```js
const byLabel = (frag) => [...document.querySelectorAll('input, textarea')]
  .find(el => {
    const l = document.getElementById(el.getAttribute('aria-labelledby') || el.id || '');
    return l && new RegExp(frag, 'i').test(l.innerText);
  });
```

Read every value back after writing, and confirm the fields you did *not* intend to
touch still hold what they held.

## Row-level Edit needs a selection

Edit and Delete on a table row stay disabled until the row's checkbox is ticked.
Delete then raises a confirmation dialog — check which row is ticked in the
screenshot before confirming.

## Dates normalise

Date inputs accept `DD/MM/YYYY` and render back as `DD-MMM-YYYY`. Reading
`07-Mar-2021` after writing `07/03/2021` confirms the interpretation, which is worth
checking on any date where the day could be read as a month.

## Session and the i18n failure

The header carries a countdown. When it expires you land on "Unauthorized" and the
taxpayer logs in again; the draft survives.

Occasionally the SPA loads without its translation strings and renders raw keys —
`common.headers.proceed_wizard`, `Common.Buttons.Skip_question`. The page still
works; match buttons on the key text, or reload through the dashboard.
