# Rock-ons

Rockstor Rock-ons that aren't quite production grade I built for my own deployment.

## How to use

As per [official docs](http://rockstor.com/docs/docker-based-rock-ons/overview.html#adding-your-own-rock-on) you can just copy the JSON files in this repo to `/opt/rockstor/rockons-metastore` on your Rockstor instance to have the definitions available.

## Why not just submit them to the official Rock-on registry?

The Rock-ons in this repo are tweaked to offer exactly the customizability I require and nothing more. This means I may have left out the configurability of some environment variables. Further the configuration flow should be streamlined for most of them to require less duplicate inputs.

## State

| rockon | usability | working |
|--------|-----------|---------|
| funkwhale | env config and user setup is a bit cumbersome | :heavy_check_mark: |
| gitea | easy - env vs. setup config a bit confusing | :heavy_check_mark: |
| huginn | not used it much yet, possibly missing an env var to add additional agents | :heavy_check_mark: |
| matrix-synapse | synapse config and database setup has to be done manually. Use the internal IP of the postgres container to tell synapse where to look (docker network bridge is your friend). Coturn can only work as a TURN server, since rockons can't map docker port ranges. I'd love to add element as a web interface, however element needs a single file mapped in. Lastly env var input is a mess due to a bug in rockstor. | :heavy_check_mark: |
| auto-invite-matrix-bot | need to manually add the config to the share. | :heavy_check_mark: |
| wallabag | submitted to the official rockon registry | :heavy_check_mark: |
