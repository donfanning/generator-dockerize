# Yeoman Docker Generator

This Yeoman Generator will add a dockerize project scaffolding to your project.

## Assumptions

* Your application lives in `./app`
* You are on `OSX`
* You are cool with `homebrew`
* Your Docker exec command is this shell script: `./app/exec`
  * sample `./app/exec`
```
#!/usr/bin/env bash
if [[ "$APP_ENV" -eq "DEV" ]]; then
  echo "dev mode, rebuild npm modules because we don't want to use osx compilations"
  npm install -s >/dev/null
  npm rebuild -s > /dev/null
  pm2 start process.dev.json && pm2 logs
else
  pm2 start process.json && pm2 logs
fi
```
* your docker app is a single web app with a memcached docker container connected to it
  * this can easily be modified after initial scaffolding by editing `docker-compose.tmpl`

## Usage

```
npm install -g yo generator-dockerize
yo dockerize
```

## Scaffolding Contents

* ./app/.rsyncignore
* ./app/exec
  * edit this after running `yo dockerize` to have instructions for running your app
  * whatever you run here should be a foreground non-exiting process
* .dockerignore
* .env
* dev
* dev.config.sh
* docker-compose.tmpl
  * this will be used to generate a per-developer docker-compose.yml, which should be added to .gitignore
  * the sync directory will be different depending on where developers clone the repo
* Dockerfile
  * you will need to update this file with your app installation instructions

# Getting Started

1. run `yo dockerize` on your project
2. edit `./app/exec` to have proper app execute instructions
3. edit `docker-compose.tmpl` if needed (probably)
4. edit the Dockerfile as needed
5. run `dev init`

The `dev` script will ensure that you have all the docker software and configuration needed to run and will run your app inside a docker container.

# HISTORY

## 1.2.0
  - add `dev vpn` to fix OSX routing table after disconnecting from Cisco AnyConnect VPN client
