# NBA Moonshot — iPhone push notifications

This feature adds the **Alerts** control and an app-specific Web Push registration to the existing GitHub Pages app.

## App identity and scope
- Installed app: **NBA Moonshot**, manifest ID `/moonshot/`, existing Moonshot icon.
- Home Screen URL: `https://mjmuntz-art.github.io/moonshot/`.
- OneSignal service worker: `/moonshot/OneSignalSDKWorker.js`.
- OneSignal service worker scope: `/moonshot/` (not the NBA PropSignal path).
- OneSignal app: existing Muntz Sports Alerts OneSignal project; public App ID in `push-config.js`.
- Moonshot preference tags: `push_moonshot_enabled`, `push_moonshot_projections`, `push_moonshot_locked`, `push_moonshot_results`.

## Deployment and safe verification
1. Merge this PR to publish the Moonshot Alerts control. No Google Apps Script code or triggers are changed.
2. Open Moonshot **from its iPhone Home Screen icon**, not a Safari browser tab.
3. Open **Alerts**, choose alert types, press **Enable notifications**, and grant the iOS permission.
4. In OneSignal **Audience → Users & subscriptions**, verify the user has `push_moonshot_enabled = 1` and the chosen event tags.
5. Send a single **manual test** in OneSignal targeted to the Moonshot subscription. Confirm the notification displays the Moonshot app identity and tapping it opens Moonshot, not NBA PropSignal or Safari. If not, stop before turning on any automatic Moonshot sending.
6. Only after successful identity verification, add a separate Moonshot GitHub Actions monitor for the existing read-only `?api=1` JSON data feed. Use a distinct checkpoint branch, `ONESIGNAL_REST_API_KEY` repository secret and a Moonshot-only enable variable. Do not modify Apps Script.

## Important
This initial PR does **not** send Moonshot notifications automatically. The Moonshot monitor and sending toggle are intentionally deferred until the iPhone identity test succeeds.

The other apps share the github.io origin. Distinct manifest IDs and separate service-worker scopes support app isolation, but **actual iPhone icon and tap routing must be verified on the user's device**, not assumed.

Never place a OneSignal REST API key in client-facing GitHub files or screenshots.
