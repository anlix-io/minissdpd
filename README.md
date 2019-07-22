## OpenWRT instructions

Add the following line into "feeds.conf.default" (You will find it under the top dir of your workspace).

    src-git minissdpd https://github.com/anlix-io/minissdpd-feeds;master

Then execute:

    scripts/feeds update -f minissdpd
    scripts/feeds install -a -p minissdpd
