# TRS test documents — hosting

Twenty page images for the sixteen test documents behind the Cognigy form-completeness demo,
plus `manifest.json` and a browsable `index.html`.

## What to do with this folder

1. Publish it over HTTPS at a stable path. The existing NiCE demo asset host is the natural home:
   copy this folder to `demo-assets/trs-forms/` in the `jonmccain-nice.github.io` repo and push.
2. Open `<base>/index.html` and confirm the thumbnails load.
3. In the Cognigy flow, open the **Config** Code Node and set `documentBaseUrl` to the folder URL,
   with a trailing slash:

   ```
   documentBaseUrl: 'https://jonmccain-nice.github.io/demo-assets/trs-forms/'
   ```

That is the only change needed. `Resolve Document` builds every page URL from that one value.

## Why the images are hosted rather than carried in the flow

The vision model fetches each page itself from the URL in the request. No image bytes pass through
Cognigy, which matters: Cognigy silently truncates any single context or input value above roughly
64K characters, and one legible form scan is 300K or more as base64. Hosting the pages keeps the
request body around 6K regardless of page count, and it is also how the production FileNet path
should work — FileNet issues a retrieval URL, Cognigy passes it along.

The images must be publicly readable for this to work. For a production deployment against real
member documents, that is not acceptable, and the retrieval URL needs to be a short-lived signed
one that the model can fetch once. See the blueprint's Implementation Checklist.

## Files

| File | What it is |
|---|---|
| `FN-*.jpg` | Scanned pages, grayscale JPEG, 1600px wide. `-p1`, `-p2` suffix is page order. |
| `manifest.json` | documentId to page-file mapping. Mirrors the registry in the Resolve Document node. |
| `index.html` | Browsable gallery with the scoresheet verdict per document. |

These are the same test documents TRS supplied, converted from PDF to the JPEG form FileNet stores.
Nothing was retouched — the corrections, stray marks and stamps in the originals are all intact,
which is the point.
