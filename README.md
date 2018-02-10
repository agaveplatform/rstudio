# Agave RStudio Server

Power your digital lab and reduce the time from theory to discovery using the
Agave Science-as-a-Service API Platform. Agave provides hosted services that
allow researchers to manage data, conduct experiments, and publish and share
results from anywhere at any time.

## Overview

The Agave RStudio Server image is built from the popular
[rocker/rstudio:3.4.3](https://hub.docker.com/r/rocker/rstudio/) image. The image is enhanced with preconfigured installation of the Agave [CLI](https://github.com/agaveplatform/agave-cli, Python ([agavepy](https://github.com/TACC/agavepy)) and R ([rAgave](https://github.com/agaveplatform/r-sdk)) SDK, and installers to automate the synchronization of notebooks from Agave's ["AP" series of tutorials](https://github.com/agaveplatform/notebooks).

> For more
information on customizing the RStudio Server environment can be found in their [Github repository](https://github.com/rocker-org/rocker/wiki/Using-the-RStudio-image).

### Prerequisites

The only requirement is [Docker](https://docs.docker.com/engine/installation/) and [Docker Compose](https://docs.docker.com/compose/install/). Please consult the official Docker website for installation instructions for your system.

## Building

T build the image, you have two options. First is to run the Docker build
command manually:

```Bash
docker build -it --rm -t rstudio:2.4.3 .
```

Second is to use the `docker-compose.yml file to do it for you.  

```
docker-compose build
```

## Using the image

This repository includes a `docker-compose.yml` file, which will start up your
RStudio Server with some sensible defaults an create a persistent data volume
to preserve your session data to your docker host and make it available between
runs.

To start your RStudio Server, run the following command from the repository root directory.
cd to the project folder and

```bash
$ docker-compose up -d
```  

The first time you run the command, the Docker image will be downloaded to your
host machine. This can take a while depending on the speed of your internet
connection.

Once started, point your browser to http://<your docker hostname>:8787 and
access the RStudio server. Login using:  

> * username: rstudio  
> * password: rstudio  

### Environment variables

Upon startup, the server will search the environment for any variables that begin
with `AGAVE_`. Any variables it finds will be copied to your `~/.Renviron` file
and made available in your RStudio session environment. Thus, by setting the
AGAVE_USERNAME, AGAVE_PASSWORD, and AGAVE_TENANT variables in your container
environment, you will be able to start interacting with the Agave Platform
without having to deal with authentication, client key management, etc. in the
CLI or any of the Agave SDK you use.

The docker compose file included in this repository will look for the
AGAVE_USERNAME, AGAVE_PASSWORD, and AGAVE_TENANT environment variables in your
local systme environment and attempt to populate the container's environment
with those values. To change this, or add any additional variables, you can edit
the `docker-compose.yml` file. The full list of variables supported is given
below.  

| Name                                        | Description                                                                                                                      |
|---------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| =======================================     | ==============================                                                                                                   |
| AGAVE_ACCESS_TOKEN                          | OAuth2 bearer token returned from the tokens API                                                                                 |
| AGAVE_BASE                                  | Base URL to the tenant to which you would like to connect.                                                                       |
| AGAVE_CACHE_DIR                             | The irectory where the auth cache file created by the CLI an SDK should be stored. Defaults to $HOME/.agave                      |
| AGAVE_CACHE_VAR                             | The prefix used on all keys when caching session data.                                                                           |
| AGAVE_CLIENT_KEY                            | The API client key used to obtain and refresh auth tokens.                                                                       |
| AGAVE_CLIENT_SECRET                         | The API client secret used to obtain and refresh auth tokens.                                                                    |
| AGAVE_CLI_COMPLETION_CACHE_APPS             | Truthy variable indicating whether calls to the Apps API should be cached.                                                       |
| AGAVE_CLI_COMPLETION_CACHE_CLIENTS          | Truthy variable indicating whether calls to the Clients API should be cached.                                                    |
| AGAVE_CLI_COMPLETION_CACHE_FILES            | Truthy variable indicating whether calls to the Files API should be cached.                                                      |
| AGAVE_CLI_COMPLETION_CACHE_JOBS             | Truthy variable indicating whether calls to the Jobs API should be cached.                                                       |
| AGAVE_CLI_COMPLETION_CACHE_LIFETIME         | Duration in seconds that the cache should remain valid.                                                                          |
| AGAVE_CLI_COMPLETION_CACHE_METADATA         | Truthy variable indicating whether calls to the Metadata API should be cached.                                                   |
| AGAVE_CLI_COMPLETION_CACHE_MONITORS         | Truthy variable indicating whether calls to the Monitors API should be cached.                                                   |
| AGAVE_CLI_COMPLETION_CACHE_NOTIFICATIONS    | Truthy variable indicating whether calls to the Notifications API should be cached.                                              |
| AGAVE_CLI_COMPLETION_CACHE_POSTITS          | Truthy variable indicating whether calls to the PostIts API should be cached.                                                    |
| AGAVE_CLI_COMPLETION_CACHE_PROFILES         | Truthy variable indicating whether calls to the Profiles API should be cached.                                                   |
| AGAVE_CLI_COMPLETION_CACHE_SYSTEMS          | Truthy variable indicating whether calls to the Systems API should be cached.                                                    |
| AGAVE_CLI_COMPLETION_CACHE_TAGS             | Truthy variable indicating whether calls to the Tags API should be cached.                                                       |
| AGAVE_CLI_COMPLETION_CACHE_TENANTS          | Truthy variable indicating whether calls to the Tenants API should be cached.                                                    |
| AGAVE_CLI_COMPLETION_CACHE_TRANSFORMS       | Truthy variable indicating whether calls to the Transforms API should be cached.                                                 |
| AGAVE_CLI_COMPLETION_CACHE_TRANSFERS        | Truthy variable indicating whether calls to the Transfers API should be cached.                                                  |
| AGAVE_CLI_COMPLETION_SHOW_FILES             | Truthy variable indicating whether completions should be shown on the files API.                                                 |
| AGAVE_CLI_COMPLETION_SHOW_FILE_IMPORTS      | Truthy variable indicating whether completions should be shown when specifying the source parameter of the files import command. |
| AGAVE_CLI_COMPLETION_SHOW_FILE_PATHS        | Truthy variable indicating whether completion should be shown when building file paths.                                          |
| AGAVE_CLI_COMPLETION_SHOW_JOB_OUTPUTS_PATHS | Truthy variable indicating whether completion should be shown when building job output paths.                                    |
| AGAVE_CLI_HOME                              | Location of the AGAVE cli base directory.                                                                                        |
| AGAVE_DEVEL_MODE                            | Should requests be made against a development version of the Agave Platform.                                                     |
| AGAVE_DEVURL                                | URL of the development Agave Platform services.                                                                                  |
| AGAVE_DEV_TENANTS_API_BASEURL               | URL of the development Agave Platform tenants API.                                                                               |
| AGAVE_ENABLE_AUTO_REFRESH                   | Should                                                                                                                           |
| AGAVE_JSON_PARSER                           | Which JSON parser should the CLI use. One of jq, python, json-mirror, native. Defaults to jq.                                    |
| AGAVE_PASSWORD                              | The password used to login to the tenant.                                                                                        |
| AGAVE_TENANT                                | The code of the tenant to which requsts will be made. Defaults to "agave.prod"                                                   |
| AGAVE_TENANTS_API_BASEURL                   | URL of the Agave Platform tenants API.                                                                                           |
| AGAVE_USERNAME                              | The usernamed used to login to the tenant.                                                                                       |

### Quickstarts  

After logging in to your RStudio server, you will see an `example` folder in
your home directory with several Rmarkdown files in it. Each of these is a
quickstart tutorial on a specific topic around using the Agave Platform and,
specifically, the Agave R SDK, "rAgave." For first time users, the
`R-quickstart.Rmd` file is the best place to begin.

## References  

* [Agave Platform](https://agaveapi.co/)  
* [Docker Hub Page](https://hub.docker.com/r/agaveplatform/rstudio)  
* [RStudio webite](https://www.rstudio.com/)  

## License

The Agave Platform and rAgave are licensed under the [BSD 3-Clause license](LICENSE).
