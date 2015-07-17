## Get webhook with jekyll-hook

Github provides us webhook for each repository, it allows us to build or set up integrations which subscribe to certain events on Github.com. When one of these event below is triggered, github will send a HTTP POST payload to the webhook's configured URL. What we want to do is when our repository gets a new "push", the server receives a push event and it deploys automatically the newest version of our project on the server.

| name | Description |
| --- | --- |
| *	| Any time any event is triggered.|
| commit_comment | Any time a Commit is commented on. |
| create	| Any time a Branch or Tag is created.|
| delete	| Any time a Branch or Tag is deleted.|
| deployment | Any time a Repository has a new deployment created from the API.|
| deployment_status | Any time a deployment for a Repository has a status update from the API. |
| fork | Any time a Repository is forked. |
| gollum | Any time a Wiki page is updated. |
| issue_comment | Any time an Issue or Pull Request is commented on. |
| issues | Any time an Issue is assigned, unassigned, labeled, unlabeled, opened, closed, or reopened. |
| member | Any time a User is added as a collaborator to a non-Organization Repository. |
| membership | Any time a User is added or removed from a team. Organization hooks only. |
| page_build | Any time a Pages site is built or results in a failed build. |
| public | Any time a Repository changes from private to public. |
| pull_request_review_comment | Any time a comment is created on a portion of the unified diff of a pull request (the Files Changed tab). |
pull_request	Any time a Pull Request is assigned, unassigned, labeled, unlabeled, opened, closed, reopened, or synchronized (updated due to a new push in the branch that the pull request is tracking).
push	Any Git push to a Repository, including editing tags or branches. Commits via API actions that update references are also counted. This is the default event.
repository	Any time a Repository is created. Organization hooks only.
release	Any time a Release is published in a Repository.
status	Any time a Repository has a status update from the API
team_add	Any time a team is added or modified on a Repository.
watch	Any time a User watches a Repository.