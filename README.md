# proto-static-website

A static prototype website for the Recovery Supervision Service.

## Pages

| File | Description |
|------|-------------|
| `index.html` | Main landing page |
| `page2.html` | Stand-alone post-submission confirmation page |
| `style.css` | Shared stylesheet |

## "Pay and Subscribe" button & Google Form

Clicking the **Pay and Subscribe** button on `index.html` opens an embedded Google Form inside a modal dialog. The form captures lead interest (subscription intent and willingness-to-pay).

### Changing the Google Form URL

The form URL is stored in a single JavaScript variable at the bottom of `index.html`:

```js
var FORM_URL_EMBEDDED = 'https://docs.google.com/forms/d/e/<FORM_ID>/viewform?embedded=true';
```

Replace `<FORM_ID>` (and the surrounding path) with the ID of your own Google Form. `FORM_URL_DIRECT` is derived automatically by stripping `?embedded=true`, so it is used for the "Open in new tab" fallback — **you only need to update the one variable**.

### Post-submit confirmation behaviour

Because Google Forms are served from a different origin (`docs.google.com`), the browser blocks JavaScript from reading the iframe's URL after the form is submitted. The page therefore uses a **two-step pragmatic UX**:

1. **Automatic detection (best-effort):** An `iframe` `load` event listener tries to read `contentWindow.location.href`. If the page has navigated to the Google Form's thank-you URL (which contains `formResponse`), the modal is closed automatically and both confirmation messages are revealed.
2. **Manual confirmation (fallback):** A prominent green **"I submitted the form ✓"** button is always visible inside the modal. After submitting the form, the user clicks this button to dismiss the modal and see both messages.

Both of the following messages are displayed together after confirmation:
- **Subscribed:** "Thank you for your interest… We will keep you informed…"
- **Payment interest noted:** "I am not in a state to accept a payment but thank you for considering it."

### Keyboard accessibility

- The modal is a `<div role="dialog" aria-modal="true" aria-labelledby="modalTitle">`.
- Opening the modal moves focus to the confirmation button.
- Pressing **Escape** closes the modal and returns focus to the **Pay and Subscribe** button.
- Clicking outside the modal (on the dark overlay) also closes it.
- All interactive elements have visible `:focus-visible` outlines.
