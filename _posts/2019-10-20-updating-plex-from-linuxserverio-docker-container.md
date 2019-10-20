---
layout: post
title:  "Updating Plex from linxserver.io docker container"
date:   2019-10-20 01:58:00 -0400
categories: nginx ssl letsencrypt
---

This is an SOP for updating a plex instance created from the linuxserver.io plex image.

## Pre-requisites
This SOP assumes you have a docker file, docker-compose.yml, or a script with a docker run command ready to go. If this isn't the case, go do that.

## Pull the latest version of the linuxserver.io plex instance
```
docker pull linuxserver/plex
```

Your local repository will now contain the latest linuxserver/plex image. The next step is to kill the running plex container(s) and recreate it using the new image. If using docker-compose this can be done with `docker compose up -d`, otherwise build your docker image via. the file and run the container. If you have a launch script, this would be the time to execute it.

The container should now be running the latest version of the plex image, verify this by launching plex and bask in the glory of no longer having to see yellow out of date plex version banner.

// END SOP



