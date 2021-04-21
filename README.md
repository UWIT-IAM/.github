# .github
This is where org-wide github templates and other things must be stored.


## Github Actions Secrets

When using Github Actions, there are a few organization-wide secrets at your disposal:

- `GCR_TOKEN`: A service account credential that is allowed to push to and pull from our GCR repositories.
- `IAM_GCR_REPO`: The name of our GCR project id; this isn't necessarily a secret by itself, but we should endeavor to keep it out of public contexts
- `ACTIONS_SLACK_BOT_TOKEN`: Token for the slack bot that is allowed to post from Github actions.

To use these secrets, simply include them from the secrets context in your github actions and workflows:

```yaml
env:
   repo_url: gcr.io/${{ secrets.IAM_GCR_REPO }}/app-name
   
```

These are available to any repository inside the UWIT-IAM organization.
