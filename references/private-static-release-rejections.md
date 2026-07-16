# Private static-release rejection patterns

Use this when the fixed private flow reaches `POST /deployment` and returns HTTP 422.

## Upload transport and filename

The upload endpoint expects the ZIP as **raw binary**, not `multipart/form-data`:

```bash
curl --fail --show-error --data-binary @"$ZIP_PATH" \
  -H 'Content-Type: application/zip' \
  -H "X-File-Name: $ZIP_FILENAME" \
  "https://coze-js-api.devtool.uk/file-transfer/upload?filename=$ZIP_FILENAME"
```

A multipart upload can be stored as a `.bin` object. `/deployment` rejects a URL that is not a controlled HTTPS `.zip` URL with `UNTRUSTED_ARTIFACT_URL`.

## Release-safe asset paths

The public target is an immutable release subpath. Convert page-local root paths such as `/assets/app.js` and `/products/image.jpg` to relative paths such as `./assets/app.js` and `./products/image.jpg` before packaging. Verify every `src`/`href` local resource against the final release URL, not only locally at `/`.

## File allowlist is stricter than generic static hosting

Do not include host-specific metadata such as `site/_headers`. A deployment can reject it with:

```json
{
  "status": "rejected",
  "reason": "STATIC_VALIDATION_FAILED",
  "checks": ["site 中含不允许的文件类型：site/_headers"]
}
```

Package only `site/index.html` and allowed browser-consumed assets (HTML, CSS, JS/MJS, JSON, images, fonts, media, WASM). Remove deployment-host configuration and other extensionless metadata before creating the manifest, since the manifest must exactly cover the final `site/` tree.

## Retry discipline

A 422 rejection is not a successful deployment. Read the structured `reason` and `checks`, rebuild the complete final ZIP (including a regenerated manifest and Dossier), upload it again using raw binary with a `.zip` filename, then resubmit the newly returned `data.url`. Never resubmit the prior rejected artifact URL.
