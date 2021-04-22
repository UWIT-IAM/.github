# UW-IT IAM Community guide to code reviews

Sharing and reviewing code is a great way to learn about others' code, learn new
approaches to problems, and approach a standard of code quality. Additionally, 
it's easy to spot commonly re-implemented solutions within a team that can be 
factored to something shared and commonly maintained. The more eyes invested in a change,
the more people understand the change, the system being changed, and the 
coding dialct of the person submitting, so that future (shared) maintenance
becomes easier.

Like any editorial process, peer reviews can be frictional, so it's good to have 
a common baseline. That's what this document is for. It's not a set of laws. 
It's a place to start, so that while reviewing (and being reviewed), we have a loose 
contract to use as a guide post.

If you want to take part in code reviews routinely, consider joining the 
`@iam-peer-reviewers` group, and take a look at some of the quality-of-life 
enhancements available in our github ecosystem.

#### Deep Links

- [UW-IT IAM's "Standard" of Code Review](#uw-it-iams-standard-of-code-review)
  - [An approachable contract](#an-approachable-contract-between-reviewer-and-author)
  - [Write informative commit messages](#write-informative-commit-messages)
  - [Curate your commits](#curate-your-commits)
  - [Other helpful information](#other-scenarios)

## Google's Standard of Code Review

The [Google Code Review Standard] is a fantastic reference of [what to look for], 
[how and when to review], and [writing comments] on reviews.  (CL means "changelist," 
by the way; I couldn't find it defined anywhere in the documentation.)

If a pull request ever starts becoming too frictional, use the Standard as a 
reference to help navigate. 

## UW-IT IAM's "Standard" of Code Review

Hey, I wrote this document in a couple of hours, so I don't claim for this to be a 
de facto standard, hence the air quotes. This should be community-maintained. I'll 
let whoever is the first to submit an update remove this paragraph to prove it.

### An approachable contract between reviewer and author:

Do you want to propose an amendment to this contract? That's a great use of 
the pull request medium, whether as an author or a reviewer 😄.

#### For authors:

- Prefer smaller, less impactful requests. (This makes things easier on reviewers.)
  A pull request does not need to completely close a ticket or story as long as the 
  request itself does not add additional risk to service stability. 
- To the best of your ability, review your own work under the same guidelines you
  use for others before requesting reviews. (As you get to know reviewers, you learn
  which questions they're likely to ask; use what you've learned during your own
  proofing.)
- Provide a measure of the impact of your change in the commit message; what's the 
  worst that could happen if you've gotten it wrong? (This also helps when creating 
  RFCs for releases.)
- If an amendment to your pull request (based on feedback, for example) is very 
  large, consider a second branch, with your PR branch as the target, especially if 
  you expect additional iterations of this update.  

#### For reviewers

- Prefer fewer, more thorough reviews. (This makes things easier on authors.)
  A pull request does not need to completely close a ticket or story as long as the
  request itself does not add additional risk to service stability. 
- Wait at most one business day to submit a review on a pull request iteration. 
  Beyond that the author should use their best judgment based on the importance (and 
  worst-case impact) of the change.
- Don't block on your own ignorance, but do question whether your ignorance is 
  because of the author's lack of clarity.

### Write informative commit messages

Consider a commit message the "abstract" of your change; provide context on the need 
for the change and the impact of the change. If you have links to external resouces 
that already capture this information, link that instead.

Some things to strive for:

- [ ] (Required) A summary line to introduce the change
- [ ] (Highly recommended) A narrative summary of the change
- [ ] (Often helpful) A bulleted list of noteworthy changes

```
[JIRA-123] Fix response type when input is invalid
 
Closes JIRA-123. The software was raising 500 by not properly validating 
and handling user input. This change models the input so that it can be validated, 
and returns a 400 Bad Request instead when invalid input is handled. However, the 
front-end does not currently display meaningful information; I have opened JIRA-124 
to capture that work as a follow-up task.

- Adds `FooBarInputModel` to `api/models.py`
- Creates an instance of `FooBarInputModel` when the 
  api is called and validates the response
- Adds tests for `FooBarInputModel`
- Adds tests to check for response type
```

### Curate your commits

Try to group related changes together in meaningful commits. You can always split 
and recombine commits, so  no matter your personal development workflow, take the 
time to curate and organize them before opening a pull request.

#### Tom's personal curation workflow:

NB: Providing this only as a reference for folks who are mystified by the git 
command line. Use whatever workflow you want to accomplish your goals. 
There are plenty of good reasons to do this any number of different ways.

Feel free to add your own here too, if you have something useful!

```
# First, I rebase onto the target branch
git fetch
git rebase origin/main

# Then I reset to main
git reset origin/main

# Now I can add and group related changes together so that
# they are easier to review:
git add api/methods.py api/models.py  tests/api.py
git commit -m 'Add FoodBarInputModel and raise 400 when input is invalid'
git add clients/buzz.py
git commit -m 'Add client to talk to BuzzService'
```

**Updating the pull request description is not the same as updating the commit 
message**. If you want to update the commit message, use `git commit --amend`.


### Other scenarios

#### As a reviewer

> "I don't understand enough of this to review it."

That makes you the **perfect** reviewer. That doesn't necessarily mean you should block 
the change based on your lack of understanding, but _do_ ask questions about the 
change! It might be hard for you to know what is your lack of context and what is 
the author's lack of clarity, so by simply asking questions, you learn something at 
worst, and help improve the code base at best.

> "This is so foreign to me; where do I even start?"

- Ask yourself what you _do_ understand, and start there. (Hopefully the author's 
  commit message is a great place to start.) How does what you _do_ 
  understand relate to the author's intentions? 
- Ask the author if there's a comparable example of this 
  change's pattern/framework/language/dialect you can compare to, 
  or documentation you can reference. Sometimes just knowing that there is 
  documentation you can reference when you need it is "good enough."
- Ask yourself (or the author) what is most likely to change or need maintenance, 
  and do your best to understand those components; those are what you are likely
  to interact with and see again.
  
> "I don't have time to review this today."

How much time _can_ you commit? 

- Do you have time to read the change description and commit messages? Read them.
- Does the author point out the important highlights you need to pay attention to? 
  Do you have time to read those?    
- What's the worst that can happen if you approve this change without a thorough 
  review? If the answer is "not too much, really," then you should consider 
  approving based on what you _do_ have time to review.
- Tell the author when you can review it, and honor that commitment. If the author 
  expresses that the change is blocking other work, you can decide to re-prioritize 
  or approve the change based on a less thorough review.
 
#### As an author

> "Is my code ready for review?"

Review your own code first. If possible, preview your change in a UI that isn't your 
IDE, terminal, or text editor. (Just like it's often easier to catch issues written 
in a word processor by printing a document, it helps to break out of the UI you've 
been using to view your code in a different light.) The Github Desktop app is a good 
tool for this (and is also helpful for curating your commits), or simply by opening 
a pull request in draft mode (without adding reviewers). 

Ask yourself things like:

- Did I really need that new dependency?
- Did I deviate from the norm for a good reason?
- Can this function be broken down?
- Is this comment just for me, or for everyone? (If it's just for you, it might mean 
  the code could be clearer. Try removing it.)
- Without this comment, is the code unclear? Can I rewrite the code to get rid of 
  the comment? 
- Did I add tests to cover my change? Do those tests validate what I think they do?
- Did I run those tests?
- Should I split this into multiple commits?
- Am I consistent within the context of this change? (Sometimes, especially with 
  large changes, you may refactor as you go, leaving inconsistent bits and bobs that 
  didn't get refactored with the same care.)
- Should I create any additional work requests (Jiras) for "next steps" 
  that will be needed after the change? Are those requests linked to this
  change in some way?

  
> "I don't have time to wait for a review."

- Is this an emergency? If so, communicate with your team to determine a faster 
  release process that may bypass a thorough review.
- If it's not an emergency, why don't you have time for it? Is it blocking your 
  other work? How much time _can_ you wait?
- Are you really blocked on all work if this change is not accepted right away?
- Have you asked anyone on your team if they can provide you with a quick review, 
  with respect to your time?
  
Unless you expect there to be some major design changes that are brought up in 
review, you can create branches from your current PR branch to continue working on 
subsequent changes. Are you really blocked?

[Google Code Review Standard]: https://google.github.io/eng-practices/review/reviewer/standard.html
[what to look for]: https://google.github.io/eng-practices/review/reviewer/looking-for.html
[how and when to review]:  https://google.github.io/eng-practices/review/reviewer/navigate.html
[writing comments]: https://google.github.io/eng-practices/review/reviewer/comments.html
