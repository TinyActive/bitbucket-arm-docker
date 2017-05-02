![Atlassian Bitbucket Server](https://www.atlassian.com/dam/wac/legacy/bitbucket_logo_landing.png)

Bitbucket Server is an on-premises source code management solution for Git that's secure, fast, and enterprise grade. Create and manage repositories, set up fine-grained permissions, and collaborate on code - all with the flexibility of your servers.

Learn more about Bitbucket Server: <https://www.atlassian.com/software/bitbucket/server>

# Overview

This Docker container makes it easy to get an instance of Bitbucket up and running.

** We strongly recommend you run this image using a specific version tag instead of latest. This is because the image referenced by the latest tag changes often and we cannot guarantee that it will be backwards compatible. **

# Quick Start

For the `BITBUCKET_HOME` directory that is used to store the repository data
(amongst other things) we recommend mounting a host directory as a [data volume](https://docs.docker.com/engine/tutorials/dockervolumes/#/data-volumes), or via a named volume if using a docker version >= 1.9. 

## For Bitbucket 4.12+

In Bitbucket 4.12 and later versions, volume permission is managed by entry scripts. To get started you can use a data volume, or named volumes. In this example we'll use named volumes.

    $> docker volume create --name bitbucketVolume
    $> docker run -v bitbucketVolume:/var/atlassian/application-data/bitbucket --name="bitbucket" -d -p 7990:7990 -p 7999:7999 atlassian/bitbucket-server

## For other versions

Set permissions for the data directory so that the runuser can write to it:

    $> docker run -u root -v /data/bitbucket:/var/atlassian/application-data/bitbucket atlassian/bitbucket-server chown -R daemon  /var/atlassian/application-data/bitbucket
    
Note that this command can be replaced by named volumes.

Start Atlassian Bitbucket Server:

    $> docker run -v /data/bitbucket:/var/atlassian/application-data/bitbucket --name="bitbucket" -d -p 7990:7990 -p 7999:7999 atlassian/bitbucket-server

**Success**. Bitbucket is now available on [http://localhost:7990](http://localhost:7990)*

Please ensure your container has the necessary resources allocated to it.
We recommend 2GiB of memory allocated to accommodate both the application server
and the git processes.
See [Supported Platforms](https://confluence.atlassian.com/display/BitbucketServer/Supported+platforms) for further information.
    

_* Note: If you are using `docker-machine` on Mac OS X, please use `open http://$(docker-machine ip default):7990` instead._

## Reverse Proxy Settings

If Bitbucket is run behind a reverse proxy server as [described here](https://confluence.atlassian.com/bitbucketserver/proxying-and-securing-bitbucket-server-776640099.html),
then you need to specify extra options to make bitbucket aware of the setup. They can be controlled via the below
environment variables.

** Note: This method of configuring CATALINA_OPTS will be going away with Bitbucket 5.0 and will be replaced with Spring Boot environment variables. **

* `CATALINA_CONNECTOR_PROXYNAME` (default: NONE)

   The reverse proxy's fully qualified hostname.

* `CATALINA_CONNECTOR_PROXYPORT` (default: NONE)

   The reverse proxy's port number via which bitbucket is accessed.

* `CATALINA_CONNECTOR_SCHEME` (default: http)

   The protocol via which bitbucket is accessed.

* `CATALINA_CONNECTOR_SECURE` (default: false)

   Set 'true' if CATALINA_CONNECTOR_SCHEME is 'https'.

# Upgrade

To upgrade to a more recent version of Bitbucket Server you can simply stop the `bitbucket`
container and start a new one based on a more recent image:

    $> docker stop bitbucket
    $> docker rm bitbucket
    $> docker run ... (See above)

As your data is stored in the data volume directory on the host it will still
be available after the upgrade.

_Note: Please make sure that you **don't** accidentally remove the `bitbucket`
container and its volumes using the `-v` option._

# Backup

For evaluations you can use the built-in database that will store its files in the Bitbucket Server home directory. In that case it is sufficient to create a backup archive of the directory on the host that is used as a volume (`/data/bitbucket` in the example above).

The [Bitbucket Server Backup Client](https://confluence.atlassian.com/display/BitbucketServer/Data+recovery+and+backups) is currently not supported in the Docker setup. You can however use the [Bitbucket Server DIY Backup](https://confluence.atlassian.com/display/BitbucketServer/Using+Bitbucket+Server+DIY+Backup) approach in case you decided to use an external database.

Read more about data recovery and backups: [https://confluence.atlassian.com/display/BitbucketServer/Data+recovery+and+backups](https://confluence.atlassian.com/display/BitbucketServer/Data+recovery+and+backups)

# Versioning

The `latest` tag matches the most recent version of this repository. Thus using `atlassian/bitbucket:latest` or `atlassian/bitbucket` will ensure you are running the most up to date version of this image.

However,  we ** strongly recommend ** that for non-eval workloads you select a specific version in order to prevent breaking changes from impacting your setup.
You can use a specific minor version of Bitbucket Server by using a version number
tag: `atlassian/bitbucket-server:4.14`. This will install the latest `4.14.x` version that
is available.


# Issue tracker

Please raise an
[issue](https://bitbucket.org/atlassian/docker-atlassian-bitbucket-server/issues) if you
encounter any problems with this Dockerfile.

# Support

For product support, go to [support.atlassian.com](https://support.atlassian.com/)