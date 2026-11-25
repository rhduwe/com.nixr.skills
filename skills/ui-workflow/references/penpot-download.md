# Download a native Penpot export

## Acceptance prerequisite

Use this procedure only after the user explicitly accepts the current design
and proposal cleanup is complete. Follow the [design acceptance rules](../SKILL.md#proposals-and-acceptance).
Do not request a native export or download an archive before that acceptance.
Temporary files, staging, proposal archives, backups and validation are not exceptions.
Inspect proposals through Penpot, prototype links and rendered image previews instead.

Use this procedure when the UI workflow needs a saved editable `.penpot` file.
The Playwright procedure passed with local Penpot 2.18.3 on 2026-11-25.
The check covered export, direct download, archive structure and authentication after a browser restart.
Read the available tool documentation before using browser or plugin capabilities.
Use the intended source file and wait for pending design saves before export.

## Default: Playwright with a persistent browser profile

Use standard Playwright with a dedicated persistent Chrome profile for native exports and downloads.
Reuse this local installation and profile:

- Playwright package: `/Users/sergei/.local/share/penpot-playwright/node_modules/playwright`
- Browser profile: `/Users/sergei/.local/share/penpot-playwright/profile`
- Penpot origin: `http://localhost:9901`

These paths describe the verified Mac setup.
On another machine, use a dedicated local directory outside the repository.
Keep the profile private because it contains authenticated browser state.
The existing parent directory has owner-only permissions (`0700`).
Do not commit, print or copy its cookies, session tokens or passwords.
Do not use the user's ordinary Chrome profile.

Open the profile through Node.js with standard Playwright:

```javascript
const { chromium } = require(
  '/Users/sergei/.local/share/penpot-playwright/node_modules/playwright'
);
const context = await chromium.launchPersistentContext(
  '/Users/sergei/.local/share/penpot-playwright/profile',
  { channel: 'chrome', headless: false, acceptDownloads: true }
);
const page = context.pages()[0] || await context.newPage();
await page.goto('http://localhost:9901/');
```

Run this code inside an async function or a Node.js REPL with top-level await.
Use only one browser process for this profile at a time.
If Penpot shows its login form, ask the user to sign in directly in that window.
Never ask the user to send credentials through chat.
Check that Penpot opens the authorized dashboard or file before requesting an export.
The profile retains the session across browser restarts.
An expired or revoked session requires another sign-in.

Reuse the installed package instead of installing dependencies for each export.
If dependencies are absent, install them outside the repository within the authorized task scope.
If browser launch fails because of sandbox permissions, use the supported approval flow.
Do not treat that failure as a Playwright download limitation.
The restricted Playwright interface in a CUA bridge does not expose every standard Playwright capability.

## Request the export and obtain its URL

Use an existing authorized signed-in session on the Penpot editor origin. The
local origin is `http://localhost:9901`; do not substitute a hosted deployment.
By default, send this request with `page.evaluate` and same-origin `fetch` in the persistent Playwright context:

```http
POST /api/rpc/command/export-binfile
Accept: text/event-stream
Content-Type: application/json

{"file-id":"<source-file-uuid>","include-libraries":false,"embed-assets":false}
```

HTTP 200 is an SSE stream, not the archive. Parse blank-line-separated events,
joining all `data:` lines in each event. Require a final `end` event; reject
`error` events and incomplete streams. The verified end event contains a Transit
URI encoded in JSON:

```text
event: end
data: {"~#uri":"http://localhost:9901/assets/by-id/<generated-uuid>"}
```

Decode the JSON and use its `~#uri` value as the archive URL. Progress messages
alone do not prove a completed export. The installed MCP plugin's
`File.export(...)` can fail with `No matching clause:` while this native route
works; report the plugin failure separately.

## Download to an accessible staging path

The returned asset URL can also require authentication: a bare unauthenticated
GET returned 401 on the local installation. Use the existing signed-in context
for the download. `curl` is suitable when an authorized authenticated API
context is already available; do not extract browser cookies or session tokens
to construct one.

Use Playwright's download event and `download.saveAs` by default.
Choose an absolute staging path separate from the canonical archive.
Refuse to overwrite a different existing staged file.
Prefer the project's temporary directory.
Record temporary paths outside the project as required by the project's rules.
The server may name the download `<uuid>.zip`.
Save the native ZIP archive with the `.penpot` extension.

After decoding the completed export URL, trigger the download in the same authenticated page:

```javascript
// exportUrl is the decoded end event URI. stagingPath is an absolute path.
const downloadReady = page.waitForEvent('download', { timeout: 30000 });
await page.evaluate(url => {
  const link = document.createElement('a');
  link.href = url;
  link.download = 'design.penpot';
  document.body.append(link);
  link.click();
  link.remove();
}, exportUrl);
const download = await downloadReady;
await download.saveAs(stagingPath);
if (await download.failure()) throw new Error('Penpot download failed');
```

Register the event listener before triggering the download.
The `saveAs` call waits for download completion.
Check the resulting archive with the procedure below.
Close the context after use with `await context.close()` to release the profile.
Keep the persistent profile and installed package for later runs.
Remove temporary staging files when they no longer serve a purpose.

### Fallbacks

Use another supported authenticated download operation or the native save dialog if standard Playwright is unavailable.
Do not replace a working Playwright download with a local receiver.

If the browser tool saves only to an inaccessible Downloads folder, a verified
alternative is an authenticated browser fetch of the archive URL followed by a
transfer of those bytes to a temporary local receiver. Bind the receiver only
to `127.0.0.1`, use an unpredictable single-use route, write only the intended
staging file, and stop the receiver after transfer. Keep credentials in the
signed-in browser; this route does not read the protected Downloads file.
Remove temporary DOM links and close only tabs created for the transfer.

## Validate and preserve

- Require a completed file, valid ZIP integrity, readable JSON entries and a
  native `manifest.json` identifying the expected Penpot source file.
- Compare file identity/revision and intended page and board identities/names
  with the current source. Record size and SHA-256. Check linked-library and
  asset coverage separately: the request flags above do not request library
  inclusion or asset embedding.
- Preserve the accepted revision and existing canonical archive until the
  staged replacement is validated and covered by the existing authorization.
  Reuse the stable canonical filename; do not accumulate revision-suffixed
  exports. For UI Foundation it is `UIValidation/Design/UIFoundation.penpot`.
- Record whether import verification was performed. A structurally valid
  download does not prove successful import, preservation of server version
  history, design acceptance, implemented-UI acceptance or device validation.

Reference: [Penpot export API discussion](https://community.penpot.app/t/download-penpot-file-via-mcp-or-api/10891/2).
