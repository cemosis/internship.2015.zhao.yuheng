## Github webhook

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
| pull_request | Any time a Pull Request is assigned, unassigned, labeled, unlabeled, opened, closed, reopened, or synchronized (updated due to a new push in the branch that the pull request is tracking). |
| push | Any Git push to a Repository, including editing tags or branches. Commits via API actions that update references are also counted. This is the default event. |
| repository | Any time a Repository is created. Organization hooks only. |
| release |	Any time a Release is published in a Repository. |
| status	| Any time a Repository has a status update from the API |
| team_add |	Any time a team is added or modified on a Repository. |
| watch | Any time a User watches a Repository.|

So what need to do is set up a server with an URL address that can receive webhooks from github, and it reads the data generated in JSON format. When there is push event, the server will deploy the csmi website with newest source code. 

## Jekyll-hook installation and setup

First install dependencies:
`sudo apt-get install git nodejs ruby ruby1.9.1-dev npm`

Symlink nodejs to node (this is a common problem with nodejs under Ubuntu)
`sudo ln -s /usr/bin/nodejs /usr/bin/node`

Install jekyll and nginx 

`sudo gem install jekyll rdiscount json`
`sudo apt-get install nginx`

To keep server running we use Forever:
`sudo npm install -g forever`

After all these steps, pull the repository and install it:
`git clone https://github.com/developmentseed/jekyll-hook.git`

`cd jekyll-hook`
`npm install`

## Configuration

Copy the default configuration file `config.sample.json` to `conf.json`
`cp config.sample.json config.json`
```json
{
    "gh_server": "github.com",
    "temp": "/home/ubuntu/jekyll-hook",
    "public_repo": true,
    "scripts": {
      "#default": {
        "build": "./scripts/build.sh",
        "publish": "./scripts/publish.sh"
      }
    },
    "secret": "",
    "email": {
        "isActivated": false,
        "user": "",
        "password": "",
        "host": "",
        "ssl": true
    },
    "accounts": [
        "developmentseed"
    ]
}
```
Configure like this way:

```json
{
    "gh_server": "github.com",
    "temp": "/home/ubuntu/jekyll-hook",
    "public_repo": true,
    "scripts": {
      "#default": {
        "build": "./scripts/build.sh",
        "publish": "./scripts/publish.sh"
      }
    },
    "secret": "",
    "email": {
        "isActivated": false,
        "user": "",
        "password": "",
        "host": "",
        "ssl": true
    },
    "accounts": [
        "solael",
        "cemosis"
    ]
}
```
The most important part is to add some github accounts in the `accounts` section, or jekyll-hook will inform you that you don't authority to deploy.

By default jekyll-hook uses `build.sh` and `publish.sh` to clone your repository, build it with jekyll and copy the generated folder `_site` to `/usr/share/nginx/html/repo_name`.

## webhook setup

In github repository settings, click `Webhooks & Services` section and enter the URL for sending payload.
`http://example.com:8080/hooks/jekyll/:branch`
In this case, i entered `http://csmi.cemosis.fr/hooks/jekyll/master`

And don't forget to choose `Just the push event` option. If other event has been sent to jekyll-hook, it may not understand and error occurs.

## Configure Nginx

To make Nginx display correctly the website, we need to change its default root path from `/usr/share/nginx/html` to `/usr/share/nginx/html/csmi.cemosis.fr/`. And to avoid link problem don't forget to remove Nginx's default index.html.
The configuration file can be found in `/etc/nginx/sites-available/default`

```
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        root /usr/share/nginx/html/csmi.cemosis.fr/;
        index index.html index.htm;

        # Make site accessible from http://localhost/
        server_name localhost;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }

```

## Make it work
To run the program, simply go into jekyll-hook's folder or using absolute path to run `.jekyll-hook.js`

But to make it continuous running， execute it with forever : `forever start jekyll-hook.js`
```
root@jekyll:/etc/nginx/sites-available# forever list
info:    Forever processes running
data:        uid  command         script         forever pid  id logfile                 uptime        
data:    [0] apSq /usr/bin/nodejs jekyll-hook.js 1240    1242    /root/.forever/apSq.log 0:2:14:17.458 

```
Forever will ensure that jekyll-hook runs all the time.

To stop it, type `forever stop 0` in console.
