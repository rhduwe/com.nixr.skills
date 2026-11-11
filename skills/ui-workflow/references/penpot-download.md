# Download a native Penpot export

## Acceptance prerequisite

Use this procedure only after the user explicitly accepts the current design
and proposal cleanup is complete. Follow the [design acceptance rules](../SKILL.md#proposals-and-acceptance).
Do not request a native export or download an archive before that acceptance.
Temporary files, staging, proposal archives, backups and validation are not exceptions.
Inspect proposals through Penpot, prototype links and rendered image previews instead.

Use this procedure when the UI workflow needs a saved editable `.penpot` file.
It was verified with the local Penpot 2.18.3 installation on 2026-10-29. Read the
available tool documentation before using browser or plugin capabilities; use
the intended source file and wait for pending design saves before export.

## Request the export and obtain its URL

Use an existing authorized signed-in session on the Penpot editor origin. The
local origin is `http://localhost:9901`; do not substitute a hosted deployment.
Send this same-origin request through a supported authenticated browser/API
capability:

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

Prefer a supported download operation that accepts an absolute output path,
or the browser's native save dialog. Stage the file outside the canonical path,
for example `/private/tmp/UIFoundation.penpot`, and refuse to overwrite a
different existing staged file. The server may name the download `<uuid>.zip`;
the native export is a ZIP archive saved with the `.penpot` extension.

If the browser tool saves only to an inaccessible Downloads folder, a verified
alternative is an authenticated browser fetch of the archive URL followed by a
transfer of those bytes to a temporary local receiver. Bind the receiver only
to `127.0.0.1`, use an unpredictable single-use route, write only the intended
staging file, and stop the receiver after transfer. Keep credentials in the
signed-in browser; this route does not read the protected Downloads file.
Remove temporary DOM links and close only tabs created for the transfer.

On the tested CUA bridge, a newly navigated tab sometimes reported CDP
unavailable while resolving a paused document response. That was a tab-specific
control limitation: normal native Chrome navigation to the same Penpot origin,
waiting for it to settle, then claiming that tab restored the documented CDP
capability. Check an existing settled signed-in tab before concluding that all
authenticated API access is blocked. Do not assume this remedy applies to
unrelated browser errors or bypass access restrictions.

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
