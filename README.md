# Library Senior Fellows at UCLA

The program is a collaborative effort of the UCLA Library, UCLA Graduate School of Education and Information Studies, and Ithaka S+R, a nonprofit that provides guidance to the academic community. This project is built using [drupal-composer](https://github.com/drupal-composer/drupal-project) with the [UC D8 distro](https://github.com/ucdavis/sitefarm_seed).

## Development Setup

### Requirements

- Keys to access private dependencies
- PHP 7 - currently `7.0.10` _(tested in Lando with 7.2.20)_
- [Composer](https://getcomposer.org/doc/00-intro.md)
- Drupal 8 environment _(see recommended Lando setup)_

### Getting Started

`git clone`, `cd [new-project-dir]`, `composer install`, and go!

#### with Lando (recommended)

- [install Lando](https://docs.devwithlando.io/installation/system-requirements.html)
- tested with Lando version: `v3.0.0-rc.16`

1. `lando start` // don't open http://senior-fellows.lndo.site/ yet!
1. `lando composer install`
1. import provided database backup or visit http://senior-fellows.lndo.site/ to complete Drupal installation
1. (if you chose to install fresh) Choose "SiteFarm Seed" installation profile
1. Database configuration

    ```txt
    Database name: drupal8
    Database username: drupal8
    Database password: drupal8
    Host: database
    ```

1. Configure site and profit!? _(note that the default theme is really blank)_
    1. Go to http://senior-fellows.lndo.site/admin/appearance and "Install and set as default" the "UCLA Gateway" theme.
    1. Continue to http://senior-fellows.lndo.site/admin/reports/status and fix the rest of your configs. 😳

### Deployment Steps

1. Put site into maint. mode – `cd <project-repo-dir>/web && drush sset system.maintenance_mode 1`
1. Make a backup – `drush ard`
1. Get updates – `cd .. && git pull`
1. Update any deps. – `composer install`
1. Rebuild Drupal's cache – `cd web && drush cr`
1. Apply any database updates – `drush updatedb -y`
1. Import config changes – `drush cim -y` 7.1 Might need to do some features based stuff here, not sure yet though
1. Rebuild Drupal's cache one more time – `drush cr`
1. Take site out of maint. mode – `drush sset system.maintenance_mode 0`

### Development Theme package update to deploy procedure

This project uses the [UCLALibrary/ucla_gateway](https://github.com/uclalibrary/ucla_gateway), this is the process for updating that theme:

1. Do theme work in UCLALibrary/ucla_gateway via fork and make PR upstream

    1a. create and push UCLALibrary/ucla_gateway tag

1. Merge PR on UCLALibrary/ucla_gateway
1. Make theme update deployment PR in project repo

    1. Fetch all project updates – `git fetch --all`
    1. Checkout topic branch – `git checkout -b deploy-<your-theme-update-topic> upstream/master`
    1. Update your composer lock – `composer update <theme-repo-package>` eg:

        - `composer update uclalibrary/ucla_gateway`
        - This will see the latest updates _(from tag created in step **1a**)_ in you theme can put them into you composer.lock file

    1. Commit changes in composer lock – `git add composer.lock && git commit -m "updates to latest theme changes with that thing`
    1. Push to your fork and make PR
    1. Approve and Merge PR and run deployment steps

## Updating Drupal Core (TBD)
