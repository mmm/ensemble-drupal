# General

This is an Ensemble formula for Drupal.

Find out more about [Ensemble](http://ensemble.ubuntu.com/).


# Usage

## Setup principia repositories

    $ mkdir principia
    $ cd principia
    $ git clone git@github.com:mmm/principia-drupal.git drupal
    $ cd ..

## Use ensemble to deploy

    $ ensemble bootstrap
    2011-06-01 08:13:24,057 INFO Bootstrapping environment 'sample' (type: ec2)
    2011-06-01 08:13:26,641 INFO 'bootstrap' command finished successfully

wait a minute or so to let EC2 catch up

    $ ensemble deploy --repository=principia drupal
    2011-06-01 08:18:43,777 INFO Connecting to environment.
    2011-06-01 08:18:46,490 INFO Formula deployed as service: 'drupal'
    2011-06-01 08:18:46,491 INFO 'deploy' command finished successfully

wait a minute or so to let EC2 catch up

    $ ensemble status
    2011-06-01 08:22:10,855 INFO Connecting to environment.
    machines:
      0: {dns-name: ec2-50-19-66-57.compute-1.amazonaws.com, instance-id: i-8d73c3e3}
      1: {dns-name: ec2-174-129-157-110.compute-1.amazonaws.com, instance-id: i-1177c77f}
    services:
      drupal:
        formula: local:drupal-201105311725
        relations: {}
        units:
          drupal/0:
            machine: 1
            relations: {}
            state: started
    2011-06-01 08:22:13,283 INFO 'status' command finished successfully

## Find the drupal instance's address

`ensemble status` tells you which instance drupal is installed on.  In the above example, drupal is installed on machine `1` or `ec2-174-129-157-110.compute-1.amazonaws.com`.

## Open in a browser to complete the Drupal install

Open a browser to

    http://<drupal instance address>/drupal6/install.php

In the above example, this would look like

    http://ec2-174-129-157-110.compute-1.amazonaws.com/drupal6/install.php


