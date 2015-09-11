# Doxygen document generation 

The first part contains the javascript code added into jekyll-webhook.js. So that the nodejs server will receive the doxygen webhooks as well as it receives jekyll webhooks.

Then there is a modified version of build.sh script to generate documentation files with cmake and doxygen.

## 

```javascript
// Receive webhook post for doxygen
app.post('/hooks/doxygen/*', function(req, res) {
    // Close connection
    res.send(202);

    // Queue request handler
    tasks.defer(function(req, res, cb) {
        var data = req.body;
        var branch = req.params[0];
        var params = [];

        // Parse webhook data for internal variables
        data.repo = data.repository.name;
        data.branch = data.ref.replace('refs/heads/', '');
        data.owner = data.repository.owner.name;

        // End early if not permitted account
        if (config.accounts.indexOf(data.owner) === -1) {
            console.log(data.owner + ' is not an authorized account.');
            if (typeof cb === 'function') cb();
            return;
        }

        // End early if not permitted branch
        if (data.branch !== branch) {
            console.log('Not ' + branch + ' branch.');
            if (typeof cb === 'function') cb();
            return;
        }

        // Process webhook data into params for scripts
        /* repo   */ params.push(data.repo);
        /* branch */ params.push(data.branch);
        /* owner  */ params.push(data.owner);

        /* giturl */
        if (config.public_repo) {
            params.push('https://' + config.gh_server + '/' + data.owner + '/' + data.repo + '.git');
        } else {
            params.push('git@' + config.gh_server + ':' + data.owner + '/' + data.repo + '.git');
        }

        /* source */ params.push(config.temp + '/' + data.owner + '/' + data.repo + '/' + data.branch + '/' + 'code');
        /* build  */ params.push(config.temp + '/' + data.owner + '/' + data.repo + '/' + data.branch + '/' + 'site');

        // Script by branch.
        var build_script = null;
        try {
          build_script = config.scripts[data.branch].build;
        }
        catch(err) {
          try {
            build_script = config.scripts['#default'].buildDoc;
          }
          catch(err) {
            throw new Error('No default build script defined.');
          }
        }
        
        var publish_script = null;
        try {
          publish_script = config.scripts[data.branch].publish;
        }
        catch(err) {
          try {
            publish_script = config.scripts['#default'].publishDoc;
          }
          catch(err) {
            throw new Error('No default publish script defined.');
          }
        }

        // Run build script
        run(build_script, params, function(err) {
            if (err) {
                console.log('Failed to build: ' + data.owner + '/' + data.repo);
                send('Your website at ' + data.owner + '/' + data.repo + ' failed to build.', 'Error building site', data);

                if (typeof cb === 'function') cb();
                return;
            }

            // Run publish script
            run(publish_script, params, function(err) {
                if (err) {
                    console.log('Failed to publish: ' + data.owner + '/' + data.repo);
                    send('Your website at ' + data.owner + '/' + data.repo + ' failed to publish.', 'Error publishing site', data);

                    if (typeof cb === 'function') cb();
                    return;
                }

                // Done running scripts
                console.log('Successfully rendered: ' + data.owner + '/' + data.repo);
                send('Your website at ' + data.owner + '/' + data.repo + ' was successfully published.', 'Successfully published site', data);

                if (typeof cb === 'function') cb();
                return;
            });
        });
    }, req, res);

});
```

## BuildDoc.sh script

```bash
#!/bin/bash
# -e: Exit immediately if a simple command exits with a non-zero status
# -x: Print a trace of simple commands and their arguments
set -e
set -x

# This script is meant to be run automatically
# as part of the jekyll-hook application.
# https://github.com/developmentseed/jekyll-hook

repo=$1
branch=$2
owner=$3
giturl=$4
source=$5
build=$6

# Check to see if repo exists. If not, git clone it
if [ ! -d $source ]; then
    git clone $giturl $source
fi

# Git checkout appropriate branch, pull latest code
cd $source
git checkout $branch
git pull origin $branch
cd -

if [ ! -d $build ]; then
	mkdir $build
fi;
cd $build

cmake $source -DCMAKE_CXX_COMPILER=/usr/bin/clang++-3.6 -DFEELPP_ENABLE_DOXYGEN=ON -DFEELPP_ENABLE_BENCHMARKS=OFF -DFEELPP_ENABLE_TESTS=OFF -DFEELPP_ENABLE_RESEARCH=OFF -DFEELPP_ENABLE_APPLICATIONS=OFF -DFEELPP_ENABLE_DOCUMENTATION=OFF -DFEELPP_ENABLE_INSTANTIATION_MODE=OFF
make doxygen
```