# Github Quality of Life Enhancements

## CODEOWNERS

CODEOWNERS is used to automatically add people to pull requests based on file paths. 
(That's all it does by itself.)

You can automatically include folks from the 
[@iam-peer-reviewers](https://github.com/orgs/UWIT-IAM/teams/iam-peer-reviewers)
group in your pull requests, and you are encouraged to do so! 

To include peer reviewers for every pull request in your repository, you can do:

```
mkdir -p .github
echo "* @UWIT-IAM/iam-peer-reviewers" >> .github/CODEOWNERS
```

That's it! If you want to further configure this, refer to Github's own CODEOWNERS 
documentation. 

If you have a primary maintainer of your code base, consider adding them as a 
CODEOWNER; you can also have maintainers of frontend vs. backend spaces, 
documentation, etc. Anyone who is likely to have a vested interest in the change to 
some space, and who has knowledge enough about the space to review changes in that 
space, should be added as a CODEOWNER.

**Note:** Codeowners are not displayed while a pull request is being drafted, so 
there is no way to know ahead of time whether anyone will be automatically 
included. This is silly, but the way things are currently.

## Actions

Github actions are a "free" way to automate your processes. Everything from code 
formatting and linting, testing, and creating and managing your pull requests 
can be automated. Actions do have a bit of a learning curve, and can be hard to 
debug when creating them from scratch, but the modularity and flexibility is pretty 
fantastic and allows for solving a variety of problems in a bunch of different ways.

We also have a shared repository of actions that may be commonly used:

- [UWIT-IAM/actions](https://github.com/UWIT-IAM/actions)

You may also like [uw-it-aca/actions](https://github.com/uw-it-aca/actions/).

### Github Actions Secrets

When using Github Actions, there are a few organization-wide secrets at your disposal:

- `GCR_TOKEN`: A service account credential that is 
  allowed to push to and pull from our GCR repositories, as well as access any 
  resources required to run our actions.
- `IAM_GCR_REPO`: The name of our GCR project id; this isn't necessarily a secret by itself, 
  but is needed often enough to be included here to avoid having to redefine it.
- `ACTIONS_SLACK_BOT_TOKEN`: 
  Token for the slack bot that is allowed to post from Github actions.

To use these secrets, simply include them from the secrets context in your github actions and workflows:

```yaml
env:
   repo_url: gcr.io/${{ secrets.IAM_GCR_REPO }}/app-name
   
```

These are available to any repository inside the UWIT-IAM organization.

## Autolink references

By autolink references links to your repositories, you can get easy-breezy hyperlinks 
to deterministic resources. 

For instance, if your repository often clears `NETID` tickets, you can set up a 
shortcut so that any time `NETID-XXX` is encountered in reviews, pull requests, etc.,
it is automatically linked to the Jira whose ID is 'NETID-XXX'. 

Thus, your commit message of `[NETID-1234] Fixes the spline reticulator` will 
automatically be displayed with a link right to the Jira when your pull request is 
viewed. Amazing!

- From your repository settings, click on "Autolink references"
- Click on "Add autolink reference"; you will be required to enter your github password.
- Fill in the prefix and link format.

For one of the IAM jiras, you can simply use (examples using the `IAM` prefix): 
Reference prefix is `IAM-`, and Target URL is `https://jira.cac.washington.
edu/browse/IAM-<num>`.

The references only support _one_ template value and that template value _must_ be a 
number.

### You can create your own links if you want

If you have something whose links are hard to standardize (like RFC wikis for 
example...), you can create short links at https://uwiam.page.link/new that adhere to 
some deterministic standard (like `uwiam.page.link/RFC-0789`), which can then be set 
up as an autoreference link (i.e., `uwiam.page.ink/RFC-<num>`).

## Docker library

We have a shared team docker library whose images are re-built every Monday morning. 
You can add to this library if you have a frequently used image pattern used in more 
than one place (or that you want others to be able to use). 

It's here: [UWIT-IAM/docker-library](https://github.com/uwit-iam/docker-library)
