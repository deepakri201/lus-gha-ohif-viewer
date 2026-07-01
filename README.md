# LUS OHIF Viewer - GitHub Actions Deployment

This repository manages GitHub Actions for deploying the custom lung ultrasound (LUS) OHIF viewer to Firebase Hosting.

## Associated repository

- [Viewers fork](https://github.com/deepakri201/Viewers) — custom OHIF viewer with usAnnotation SR export and GCP Healthcare config

## GitHub Actions

### `ohif/deploy-lus-viewer`

Builds the `Viewers` fork and deploys the static site to Firebase Hosting.

Default branch deployed: `add_gcp_and_sr`

## Configuration

Configure these in the GitHub repository **Settings → Secrets and variables → Actions**:

| Type | Name | Purpose |
|------|------|---------|
| Secret | `FIREBASE_SERVICE_ACCOUNT` | Firebase service account JSON for hosting deploy |
| Variable | `FIREBASE_PROJECT_ID` | Firebase / GCP project ID |
| Variable | `VIEWERS_REF` | Git ref to deploy (default: `add_gcp_and_sr`) |

Optional (IDC-style externalized config):

| Variable | Purpose |
|----------|---------|
| `OHIF_CONFIG_JS_URL` | URL to a Gist or raw file for `platform/app/public/config/google.js` |
| `OHIF_FIREBASE_JSON_URL` | URL to override `firebase.json` in the Viewers repo |

## Build

The workflow runs:

```bash
PUBLIC_URL=/ APP_CONFIG=config/google.js yarn --cwd platform/app run build:viewer
```

Output is deployed from `platform/app/dist` using `firebase.json` at the Viewers repo root.

## Local prerequisites (GCP)

1. Enable Cloud Healthcare API and Cloud Resource Manager API
2. Grant users `Healthcare DICOM Viewer` + `Healthcare DICOM Editor` on the DICOM store
3. Configure OAuth consent with `cloud-healthcare` scope
4. Add Firebase Hosting URL and `/callback` to OAuth client authorized origins/redirect URIs

## Manual workflow trigger

Use **Actions → ohif/deploy-lus-viewer → Run workflow** after secrets and variables are configured.
