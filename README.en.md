
# deploy

  Minimalistic deployment shell script, it run in server.

__[中文文档](README.en.md)__

__The project fork from [deploy](https://github.com/visionmedia/deploy), the original project put the shell script on the local computer, and remotely executes commands through SSH; this shell script is placed on the remote server for direct execution__

## Installation

    $ make install

  Visit the original [wiki](https://github.com/visionmedia/deploy/wiki) for additional usage information.

## Usage

      Usage: deploy [options] <env> [command]

      Options:

        -C, --chdir <path>   change the working directory to <path>
        -c, --config <path>  set config path. defaults to ./deploy.conf
        -T, --no-tests       ignore test hook
        -V, --version        output program version
        -h, --help           output help information

      Commands:

        setup                run remote setup commands
        revert [n]           revert to [n]th last deployment or 1
        config [key]         output config file or [key]
        curr[ent]            output current release commit
        prev[ious]           output previous release commit
        exec|run <cmd>       execute the given <cmd>
        list                 list previous deploy commits
        [ref]                deploy to [ref], the 'ref' setting, or latest tag

## Configuration

 By default `deploy(1)` will look for _./deploy.conf_, consisting of one or more environments, `[stage]`, `[production]`, etc, followed by directives.

    [stage]
    repo git@github.com:visionmedia/express.git
    path /var/www/myapp.com
    ref origin/master
    post-deploy /var/www/myapp.com/update.sh

## Directives

### ref (optional)

  When specified, __HEAD__ is reset to `ref`. When deploying
  production typically this will _not_ be used, as `deploy(1)` will
  utilize the most recent tag by default, however this is useful
  for a staging environment, as shown below where __HEAD__ is updated
  and set to the develop branch.

        ref origin/develop

### repo

  GIT repository to clone.

       repo git@github.com:visionmedia/express.git

### path

  Deployment path.

        path /var/www/myapp.com

## Hooks

  All hooks are arbitrary commands, executed relative to `path/current`,
  aka the previous deployment for `pre-deploy`, and the new deployment
  for `post-deploy`. Of course you may specify absolute paths as well.

### pre-deploy

      pre-deploy ./bin/something

### post-deploy

      post-deploy ./bin/restart

### test

  Post-deployment test command after `post-deploy`. If this
  command fails, `deploy(1)` will attempt to revert to the previous
  deployment, ignoring tests (for now), as they are assumed to have run correctly.

      test ./something
