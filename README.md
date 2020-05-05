# Rock-ons

Rockstor Rock-ons that aren't quite production grade I built for my own deployment.

## How to use

As per [official docs](http://rockstor.com/docs/docker-based-rock-ons/overview.html#adding-your-own-rock-on) you can just copy the JSON files in this repo to `/opt/rockstor/rockons-metastore` on your Rockstor instance to have the definitions available.

## Why not just submit them to the official Rock-on registry?

The Rock-ons in this repo are tweaked to offer exactly the customizability I require and nothing more. This means I may have left out the configurability of some environment variables. Further the configuration flow should be streamlined for most of them to require less duplicate inputs. Lastly some of the Rock-ons run unstable versions.

## State

| rockon | usability | working |
|--------|-----------|---------|
| funkwhale | env config and user setup is a bit cumbersome | :heavy_check_mark: |
| gitea | easy - env vs. setup config a bit confusing | :heavy_check_mark: |
| huginn | not used it much yet, possibly missing an env var to add additional agents | :heavy_check_mark: |
