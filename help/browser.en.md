# About the embedded browser

The embedded browser keeps third-party pages away from the rest of the app.

## Where the page runs

- The page is **embedded in the app**: the **right panel** hosts it in the *Browser* view, with its **own browser profile**, so a site's cookies and local storage never mix into your app data.
- The page **cannot reach the app or your files**: it gets no workspace paths, no settings and no agent tools.
- Traffic goes **straight to the site**. We do not proxy it, we do not log it on the way, and there is no server of ours in between.

## Links that ask for a new window

A link that asks for a new window (`target="_blank"`, or `window.open` from a script) **does not open inside the embedded browser** — the page stays where it is and nothing appears to happen. The web engine drops that request here, so we cannot show it in the same view either.

When you need one of those links:

- use **Open in system browser** in the toolbar above the page (it opens in your default browser), or
- copy the address and paste it into the address field.

## What the agent may do with this page

Browser control is **off by default**. Two things must both be true before the agent can touch a page:

1. browser control is turned on in **Settings → Browser control**, and
2. this site's origin is on the allowed list.

Once both hold, the agent can read the page text, click elements, and go back / forward / reload. Whatever it reads goes into the model context: with a local model nothing leaves your computer, but if you use a cloud provider, that provider receives it. Page text is never written to the logs in plain text.

### Typing and submitting

Typing into fields and submitting forms is a **separate switch** (also off by default) — you may want the agent to click without filling anything in.

- Every write and every submit shows a **confirmation bar in the right panel** first, with the site, the field and the text that is about to be typed. Unanswered or timed-out confirmations count as **no**.
- For typing you can choose **always allow this site**; that grant lives in the current browser session only and is dropped the moment you close the page.
- **Submitting always asks again** (there is no always-allow for it), and a form can be submitted **once per session and site**.
- Password fields, one-time codes, card numbers, and anything disabled or read-only are **refused on the page itself** — the text never reaches them.
- Only the top-level document is touched (no iframes, no shadow roots), and the text the agent types is **never written to logs**.

### Listing input fields

The agent can also **list the input fields you can see on this page** — selectors only, never the values already typed into them, and never password / one-time-code / card fields. This helps it find the field it needs to type into.

Nothing is uploaded to us: we do not collect page content, and we run no usage analytics.
