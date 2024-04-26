# PHP docker images
These images contain a mostly complete setup of PHP (including composer).
Currently these tags are available:

* PHP 8.3: `8.3`, `latest`
* PHP 8.2: `8.2`

All images contain at least a PostgreSQL client, a relatively up-to-date
version of composer and some of the most common PHP extensions. All images use
deb.sury.org for their PHP packages.

These images have been configured for a basic production setup, although
project specific tweaks of the php.ini configuration may be required. You
should still setup your PHP to run as a specific user in production
environments, see the section below on the specifics for configuring a
production image.

## Version availability and backports
Support of PHP versions is based on support at deb.sury.org. You should always
attempt to use the latest version of PHP possible.

## Image variations
All tagged images have some variations available:

* A `-dev` version with a development configuration.
* A `-debug` version with a development configuration and xdebug installed
  and setup to listen to incoming remote debug requests.

## Usage
For instructions, see the [debian](https://github.com/tweedegolf/docker-debian-image)
readme. Basic usage when using docker compose is shown below for a typical
development setup:

    services:
      app:
        image: ghcr.io/tweedegolf/php:8.3-dev
        user: "$USER_ID:$GROUP_ID"
        volumes:
          - ".:/app"
        working_dir: /app

This example will run PHP-FPM on port 9000. Note that you will still need to
run something like nginx to send your HTTP requests to PHP.

### Extending for production
When running this image under production settings, you should set a few things
up:

* Create a user that your application will run under
* Make sure that user will be the default user
* Set the `ROOT_SWITCH_USER` environment variable to your user as well to
  prevent the application from accidentaly running as root
* Copy the full application and it's dependencies into the image
* Set a working directory for PHP-FPM

See for example the Dockerfile below:

    FROM ghcr.io/tweedegolf/php:8.3
    RUN useradd -c Application -m -U app
    ENV ROOT_SWITCH_USER app
    ENV SYMFONY_ENV prod
    COPY --chown=app:app . /app
    WORKDIR /app
    USER app
