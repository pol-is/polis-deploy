# polisDeployment

If you're interested in deploying Polis, or setting up a development environment, you've come to the right place.

## Overview of system

First things first, it helps to understand a bit how the system is set up.

| Component Name | Tech | Description |
|----------------|------|-------------|
| [`polisServer`][repo-server] | Node.js | The main server. Handles client web requests (page loads, vote activity, etc.) |
| [`polisMath`][repo-math] | Clojure/JVM | The math engine.  |
| [`polisClientParticipation`][repo-participation] | Javascript | The client code for end-users. |
| [`polisClientAdmin`][repo-admin] | Javascript | The client code for administrators. |

The code in the two client-side repos compile to static assets.
We push these to s3 for CDN via `deployPreprod` and `deploy_TO_PRODUCTION`, but there is a config file for each of these projects which will allow you to configure the behavior of these scripts as described in the READMEs.
Of note though, you can use a static file server, and deploy via these scripts over `sshfs`.
Finally, for local development, these repos have hot-reload able servers you can run with the `./x` command, but note that this is not the recommended approach for serving the client assets in production.

   [repo-server]: https://github.com/pol-is/polisServer
   [repo-math]: https://github.com/pol-is/polisMath
   [repo-participation]: https://github.com/pol-is/polisClientParticipation
   [repo-admin]: https://github.com/pol-is/polisClientAdmin

### Environment variables and configuration

Each of these applications currently takes configuration from a set of environment variables.
In the future we'll be moving away from this towards configuration files, but for now, the easiest way to configure the application is to have a shared set of environment variables that you keep in a file somewhere.
You might do something like the following to set up a single secure file for these purposes:

```
mkdir ~/.polis
touch ~/.polis/envvars.sh
# set environment variables here with your text editor of choice (cough; vim)
nano ~/.polis/envvars.sh
# make sure only your user can read the directory and its contents can only be read by your user for security purposes
chmod -R og-rwx ~/.polis
# if you really want to get fancy you can encrypt the file as well...
```

Then you can run `source ~/.polis/envvars.sh` to prepare any of the servers for running.

Each of the repo READMEs should have notes on what environment variables are needed, as well as templates to start off from (please raise an issue if something is missing).
And as noted, some scripts require that configuration is in a specific file somewhere, so please refer to individual repo READMEs for full details.

Of particular note, the polisServer runs on environment variables which tell it where to look for the client repositories (host & port), and affording you a lot of flexibility in how you deploy.

TODO Compile complete starter template somewhere in this repo...


## Basic deployment

* Best place to start is the main server repo: https://github.com/pol-is/polisServer, which has instructions on setting up the database.
* Then math server: https://github.com/pol-is/polisMath
* Then build client repos and serve from some static file server...

## Docker deployment

* There's a docker-compose.yml file in https://github.com/pol-is/polisServer/blob/master/docker-compose.yml which has some instructions on running a partial dev environment with docker-compose.
* Outside contributor [**@uzzal2k5**](https://github.com/uzzal2k5) has
  created some unofficial dockerization work at [`uzzal2k5/polis_container`][polis-container].

Ultimately, it would be great if all of this content was merged into this `polis-deploy` repo.


## Contribution notes

Please help us out as you go in setting things up by improving the deployment code and documentation!

* General/system-wide issues you come across can go in https://github.com/pol-is/polis-issues/issues, and repo specific issues in their respective issues lists
* PRs improving either documentation or deployment code are welcome, but please submit an issue to discuss before making any substantial code changes
* After you've made an issue, you can try to chat folks up at https://gitter.im/pol-is/polisDeployment

## Related Resources

This section is for code and data resources related to pol.is.

- [**`audreyt/polis-tally`.**][polis-tally] Utility tool for converting
  zipped pol.is exports into a simpler CSV/JSON format.
- [**`OpenSourcePolitics/decidim-efb`.**][decidim-module] A module for
  intergrating pol.is into the Decidim participatory democracy framework.
- [**`patcon/polis-api-proxy`.**][api-proxy] API proxy for experimenting
  with different API design choices.
- [**`CivicTechTO/MyDem0cracy.ca-site`.**][mydem0cracy] A sample
  micro-site for wrapping a single Pol.is conversation.
- [**`patcon/trump-polis-twitter-bot`.**][twitter-bot] Twitter bot that creates and
  links a Pol.is conversation for each tweet by a specific user.
- [**`ForaDoEixo/wp-pol-is`.**][wp-plugin] Wordpress plugin for
  embedding Pol.is conversations.
- [**`colinmegill/conference`.**][conf-site] A sample mini-site for gathering
  reactions during multi-session live event.
- [**`padagraph/vTaiwanAirBnbImporter`.**][padagraph-importer]
  Experimental importer for bringing Pol.is export data into
[padagraph.io](http://padagraph.io).
- [**`conversa-app/conversa.cc`.**][conversa] An
  unofficial, experimental Pol.is frontend client built on the Ruby on
  Rails framework.
- [**`uzzal2k5/polis_container`.**][polis-container] Alternative docker
  setup using Docker Swarm.

   [polis-tally]: https://github.com/audreyt/polis-tally
   [decidim-module]: https://github.com/OpenSourcePolitics/decidim-efb
   [api-proxy]: https://github.com/patcon/polis-api-proxy
   [mydem0cracy]: https://github.com/CivicTechTO/MyDem0cracy.ca-site
   [twitter-bot]: https://github.com/patcon/trump-polis-twitter-bot
   [wp-plugin]: https://github.com/ForaDoEixo/wp-pol-is
   [conf-site]: https://github.com/colinmegill/conference
   [padagraph-importer]: https://github.com/padagraph/vTaiwanAirBnbImporter
   [conversa]: https://github.com/conversa-app/conversa.cc
   [polis-container]: https://github.com/uzzal2k5/polis_container
