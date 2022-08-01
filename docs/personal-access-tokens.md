# Personal Access Tokens

Most Github Actions workflows can do everything they need with the default `$GITHUB_TOKEN` environment variable. 
However, some circumstances require a "Personal Access Token" to be used.

For the convenience of the UWIT-IAM org, this is availble for all UWIT-IAM repos using the `${{ secrets.ACTIONS_PAT }}` 
github actions syntax.

This token must be scoped to a human, and must be granted the requisite permissions to do everything
needed by our Actions ecosystem.

This token should be updated when:

- New permissions are needed
- Old permissions should be deprecated
- The user the token is scoped to leaves the org
- The token is compromised

## Update the token

To update the org-wide PAT, first, generate it:

- Visit https://github.com/settings/tokens
- Click 'Generate new token'
- Name your token something useful, like: "UWIT-IAM Org-wide Actions PAT (YYYY-MM-DD)"
- Select an appropriate expiration period (or select "No expiration"
- Apply the following scopes, at a minimum. If you need to change this, please also update this documentation:

  - `repo` (all)
  - `workflow` (all)
  - `write:packages`
  - `delete:packages`
  - `read:org`
  - `read:public_key`
  - `read:user`

- Copy the generated token to your clipboard and paste it someplace safe for the moment
- Click on "Configure SSO", then select "Authorize" next to the "UWIT-IAM" label; you will be asked to sign in via UW IdP.
- Make sure the token is copied on your clipboard (in case you pasted your password in when signing in)
- Go to the IAM secrets settings: https://github.com/organizations/UWIT-IAM/settings/secrets/actions
- Next to `ACTIONS_PAT`, click `Update`, then click `Enter a new value`
- Paste your token
- Click, `save changes`
- Clear the token from your "someplace safe" where you may have pasted it.


