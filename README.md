Columbo PHP buildpack
========================

This is a Heroku buildpack which includes:
* [Ant](http://ant.apache.org/)
* [Apache](http://apache.org), including the following modules:
 * deflate, expires, headers, macro, rewrite
* [Composer](http://getcomposer.org)
* [New Relic](http://newrelic.com/)
* [NPM](https://npmjs.org/)
* [PHP](http://php.net/), including the following notable extensions:
 * apc, curl, mcrypt, memcached, mysql, mysqli, newrelic, pdo, pgsql, phar, soap, zip

This build pack should be used along with [taeram/heroku-buildpack-php-columbo-template](https://github.com/taeram/heroku-buildpack-php-columbo-template).

Requirements
============
* A [Heroku](https://www.heroku.com/) account
* An [Amazon AWS](http://aws.amazon.com/) account
* Your Amazon AWS Access Key and Secret Key
* An [Amazon S3](http://aws.amazon.com/s3/) bucket, for storing the buildpack assets

Setup
=====

Here's how to setup and configure the buildpack for the first time.

#### 1. Fork the repo

Since buildpack configuration can differ quite widely, it's a good idea to
[fork this repo](https://help.github.com/articles/fork-a-repo) and use the
fork as your buildpack.

In your Heroku application, you would simply set your `BUILDPACK_URL` as follows:
````bash
heroku config:set BUILDPACK_URL=https://github.com/your-username/heroku-buildpack-php-columbo
````

#### 2. Dependencies

If you're using Mac OS X, make sure you have `coreutils` installed:
```bash
sudo brew install coreutils
```

#### 3. Vulcan Server

First, you'll need to setup a [Vulcan](https://github.com/heroku/vulcan) server. See
[Create a Build Server](https://github.com/heroku/vulcan#create-a-build-server) step for
instructions.

#### 4. S3CMD

Next, you'll need to install and configure s3cmd:
```bash
sudo apt-get -y install s3cmd
s3cmd --configure
```

If you haven't created an S3 bucket yet, you can do that now using s3cmd:
```bash
s3cmd mb s3://my-bucket-name
```

#### 5. Build all the things

You'll need to tell the buildpack what S3 bucket you're using:
```bash
cat > ./support/config.sh
BUILDPACK_S3_BUCKET=my-bucket-name
```

Finally, we're ready to compile the buildpack assets:
```bash
./support/vulcan.sh all
```

You can also build individual assets to save time:
```bash
./support/vulcan.sh php newrelic
```

#### 6. Updating your buildpack assets

At some point down the road, you may decide you want more recent versions of the
buildpack assets. Simply update the versions in `variables.sh`, and re-run
`./support/vulcan.sh` as detailed above.

Meta
----

This repo is a fork of [heroku/heroku-buildpack-php](https://github.com/heroku/heroku-buildpack-php),
and includes code from [heroku/heroku-buildpack-nodejs](https://github.com/heroku/heroku-buildpack-nodejs)
and [heroku-buildpack-php-tyler](https://github.com/iphoting/heroku-buildpack-php-tyler).
