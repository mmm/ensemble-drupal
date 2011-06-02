# Principia Drupal

This is an Ensemble formula for Drupal.

Find out more about [Ensemble](http://ensemble.ubuntu.com/).


# Installation

## Setup a formula repository

Create a formula repository called `principia` and clone this repo into it:

    $ mkdir principia
    $ cd principia
    $ git clone git@github.com:mmm/principia-drupal.git drupal
    $ cd ..

## get the principia mysql formula

Pull the principia mysql formula

    $ cd principia
    $ bzr branch lp:~ensemble-composers/principia/oneiric/mysql/trunk mysql
    $ cd ..


# Usage

## Use ensemble to deploy

### bootstrap

    $ ensemble bootstrap
    2011-06-01 19:57:50,219 INFO Bootstrapping environment 'sample' (type: ec2)
    2011-06-01 19:58:02,297 INFO 'bootstrap' command finished successfully


### start `mysql` and `drupal`

wait a minute or so to let EC2 catch up

    $ ensemble deploy --repository=principia mysql
    2011-06-01 20:01:03,265 INFO Connecting to environment.
    2011-06-01 20:01:11,617 INFO Formula deployed as service: 'mysql'
    2011-06-01 20:01:11,619 INFO 'deploy' command finished successfully

wait a minute or so to let EC2 catch up

    $ ensemble deploy --repository=principia drupal
    2011-06-01 20:01:17,186 INFO Connecting to environment.
    2011-06-01 20:01:25,228 INFO Formula deployed as service: 'drupal'
    2011-06-01 20:01:25,254 INFO 'deploy' command finished successfully

wait a minute or so to let EC2 catch up

    $ ensemble status
    2011-06-01 20:06:36,383 INFO Connecting to environment.
    machines:
      0: {dns-name: ec2-75-101-228-56.compute-1.amazonaws.com, instance-id: i-4507b52b}
      1: {dns-name: ec2-50-19-2-206.compute-1.amazonaws.com, instance-id: i-7108ba1f}
      2: {dns-name: ec2-174-129-114-51.compute-1.amazonaws.com, instance-id: i-ed08ba83}
    services:
      drupal:
        formula: local:drupal-201105311725
        relations: {}
        units:
          drupal/0:
            machine: 2
            relations: {}
            state: started

      mysql:
        formula: local:mysql-81
        relations: {}
        units:
          mysql/0:
            machine: 1
            relations: {}
            state: started
    2011-06-01 20:06:44,037 INFO 'status' command finished successfully

### Connect the two services

Once both the `mysql` and the `drupal` services both show a `started` status,
add the relation between the two to activate and couple the services.

    $ ensemble add-relation mysql drupal
    2011-06-01 20:13:15,326 INFO Connecting to environment.
    2011-06-01 20:13:21,644 INFO Added mysql relation to all service units.
    2011-06-01 20:13:21,645 INFO 'add_relation' command finished successfully
 
now status should show

    $ ensemble status
    2011-06-01 20:15:53,648 INFO Connecting to environment.
    machines:
      0: {dns-name: ec2-75-101-228-56.compute-1.amazonaws.com, instance-id: i-4507b52b}
      1: {dns-name: ec2-50-19-2-206.compute-1.amazonaws.com, instance-id: i-7108ba1f}
      2: {dns-name: ec2-174-129-114-51.compute-1.amazonaws.com, instance-id: i-ed08ba83}
    services:
      drupal:
        formula: local:drupal-201105311725
        relations: {db: mysql}
        units:
          drupal/0:
            machine: 2
            relations:
              db: {state: up}
            state: started
      mysql:
        formula: local:mysql-81
        relations: {db: drupal}
        units:
          mysql/0:
            machine: 1
            relations:
              db: {state: up}
            state: started
    2011-06-01 20:16:06,405 INFO 'status' command finished successfully



## Find the drupal instance's address

`ensemble status` tells you which instance drupal is installed on.  In the above example, drupal is installed on machine `2` or `ec2-174-129-114-51.compute-1.amazonaws.com`.

## Open in a browser to complete the Drupal install

Open a browser to

    http://<drupal instance address>/drupal6/install.php

In the above example, this would look like

    http://ec2-174-129-114-51.compute-1.amazonaws.com/drupal6/install.php


