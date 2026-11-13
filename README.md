# renovate-config

NimbleBrain's shared [Renovate](https://docs.renovatebot.com/) policy, run by the Mend Renovate app.

## Files

- `default.json` is the shared preset. It holds policy: the Monday-morning schedule in
  Honolulu time, the 14-day wait before a release is proposed, a monthly lock-file refresh schedule (off
  unless the repo turns it on, see `default.json`),
  dashboard approval for majors, and security fixes that skip all of those.
- `org-inherited-config.json` is read by the Mend app for every repo in the org. It sets only
  the config a new repo's onboarding PR proposes, so new repos start on the shared preset.

## Using it in a repo

A repo's `renovate.json` extends the preset and adds its own scope:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>NimbleBrainInc/renovate-config"],
  "description": "Why these managers, and anything this repo does differently.",
  "enabledManagers": ["npm", "github-actions"]
}
```

Policy lives here; scope lives in the repo. Each repo lists its `enabledManagers` and its
package groups, because only the repo knows which of its dependencies are safe to move on a
schedule. A repo overrides a policy setting by setting it again, with a `description` saying
why (for example, `"minimumReleaseAge": null` for images on a registry that publishes no
release dates, where the wait would hold every update forever).

The Mend app reads this repo only if it is installed on it.
