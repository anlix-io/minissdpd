## OpenWRT instructions

Add the following line into "feeds.conf.default" (You will find it under the top dir of your workspace).

    src-git miniupnp https://github.com/anlix-io/miniupnp-feeds;master

Then execute:

    scripts/feeds update -f miniupnp
    scripts/feeds install -a -p miniupnp
