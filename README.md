# Rock-ons

Rockstor Rock-ons that aren't quite production grade I built for my own deployment.

## How to use

As per [official docs](http://rockstor.com/docs/docker-based-rock-ons/overview.html#adding-your-own-rock-on) you can just copy the JSON files in this repo to `/opt/rockstor/rockons-metastore` on your Rockstor instance to have the definitions available.

## Why not just submit them to the official Rock-on registry?

The Rock-ons in this repo are tweaked to offer exactly the customizability I require and nothing more. This means I may have left out the configurability of some environment variables. Further the configuration flow should be streamlined for most of them to require less duplicate inputs.

## State

| rockon | usability | working | upstreamed |
|--------|-----------|---------|------------|
| funkwhale | env config and user setup is a bit cumbersome | :heavy_check_mark: |
| forgejo | easy - env v.s setup config a bit confusing | :heavy_check_mark: |
| gitea | easy - env vs. setup config a bit confusing | :heavy_check_mark: |
| huginn | not used it much yet, possibly missing an env var to add additional agents | :heavy_check_mark: |
| matrix-synapse | synapse config and database setup has to be done manually. Use the internal IP of the postgres container to tell synapse where to look (docker network bridge is your friend). Coturn can only work as a TURN server, since rockons can't map docker port ranges. I'd love to add element as a web interface, however element needs a single file mapped in. Lastly env var input is a mess due to a bug in rockstor. Restarting the rockon fails if the db container was updated with watchtower. | :heavy_check_mark: |
| auto-invite-matrix-bot | need to manually add the config to the share. | :heavy_check_mark: |
| wallabag | submitted to the official rockon registry | :heavy_check_mark: | ⏲️ |
| dnscrypt-proxy | requires configuration file to be manually created in config share for best experience. | :heavy_check_mark: |
| vikunja | Uses sqlite, so not well suited for multi-user operation. In order to create an initial account, registration has to be allowed, after that you have to re-create the rockon with registration forbidden if you don't want strangers to be able to sign up | :heavy_check_mark: |
| webthings-gateway | Port entry in wizzard is just for the UI link in rockstor. The container runs in host network mode, so if you want the container to bind to different ports, use local.json, as described in the container readme. If you want to use lbuetooth or zigbee, you'll have to modify the rockon definition accordingly. | :heavy_check_mark: |
| mautrix-telegram | Pretty simple to set up, with good documentation. However, requires linking a file to the synapse homeserver's container. | :heavy_check_mark: |
| mautrix-whatsapp | Pretty simple to set up, with good documentation. However, requires linking a file to the synapse homeserver's container. | :heavy_check_mark: |
| mautrix-signal | Pretty simple to set up, with good documentation. However, requires linking a file to the synapse homeserver's container. | :heavy_check_mark: |
| synapse-admin | Simple setup, rock-on candidate, but useless without matrix-synapse rock-on. | :heavy_check_mark: |
| gotify | Simple to use, [PR against registry](https://github.com/rockstor/rockon-registry/pull/287) | :heavy_check_mark: | :heavy_check_mark: |
| renovate | Extremely simple setup, but relies on a config file generated by the CLI tool. | :heavy_check_mark: |
| drone | Set up for usage with the gitea/forgejo rockon, provides a single runner. | :heavy_check_mark: |
| chrony | Super simple if you know a friendly neighbourhood NTP server to chain from | :heavy_check_mark: |
| zigbee2mqtt (+mosquitto) | Needs an initial config file to be manually generated. | :heavy_check_mark: |
| nextcloud-plus | Built to migrate from an existing nextcloud official + postgres setup to this. | :heavy_check_mark: |
| up-common-proxies | Simple to set up. | :heavy_check_mark: |
| calibre | | :heavy_check_mark: |
| synatainer | SImple to configure via env vars. | :heavy_check_mark: |
| bonob | Config via env vars, customized to what my setup needs. | :heavy_check_mark: |
| ntfy | Configuration via config file recommended. | :heavy_check_mark: |
| mosquitto | | :heavy_check_mark: |
| homeassistant-zigbee | Same as the official one, just also has a zigbee serial device config option. | :heavy_check_mark: |
| hassio-thread-matter | Medium, but might just work... | :heavy_check_mark: |
| dmarc-report-viewer | Needs a bunch of env vars that get set, but very easy other than that. | :heavy_check_mark: |
