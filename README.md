# PUDL Examples

This repository contains a collection of
[Jupyter notebooks](https://jupyter.org) with examples of how to use the data
and software distributed under [Catalyst Cooperative](https://catalyst.coop)'s
[Public Utility Data Liberation (PUDL) project](https://github.com/catalyst-cooperative/pudl).

The example notebooks depend on having the processed PUDL data available, and
it's too large to commit to a GitHub repository. There are two main ways to
access it. You can either download it to your computer and run our Docker
container locally, or you can request an account on
[our JupyterHub](https://catalyst-cooperative.pilot.2i2c.cloud/) which is
hosted in collaboration with [2i2c.org](https://2i2c.org).

## Option 1: Download the processed data and run Docker
* Download and extract this archive (~6 GB) from Zenodo (ADD LINK) into a
  local directory. On MacOS and Windows you should just be able to double-click
  the archive file. On Linux (or MacOS) you may want to use the command line:
  ```
  tar -xzf databeta-YYYY-MM-DD.tgz
  ```
  (where `YYYY-MM-DD` is a date string). It may take a couple of minutes to
  extract.

* Extracting the archive will create a directory called `databeta-YYYY-MM-DD`
  containing the example Jupyter Notebooks from this repository, and all the
  processed PUDL data as a combination of [SQLite](https://www.sqlite.org)
  databases and [Apache Parquet](https://parquet.apache.org/) files.

* If you have additional data you want to work with, you can put it in the
  `user_data` directory, and it will be available to you inside the Docker
  container for use in your own notebooks. You can also save outputs to that
  directory inside the Docker container, and they will be available in the
  `user_data` directory on your computer.

* Now you need to [install Docker](https://docs.docker.com/get-docker/).
  On MacOS and Windows it'll be called "Docker Desktop". On Linux it's just
  "Docker."

* On Linux, you'll need to separately install a tool called
  [docker-compose](https://docs.docker.com/compose/install/) (it comes
  bundled with Docker Desktop for MacOS/Windows).

* If you're on MacOS or Windows, open the settings in Docker Desktop and
  increase the amount of memory that Docker is allowed to use to at least 8GB.

* Check to make sure that the Docker service is running in the background. On
  MacOS it should show up in the menu bar. On Windows it should show up in the
  system tray. On Linux, a daemon called `dockerd` should be running in the
  background.

* At a command line, go into the directory which was created by extracting the
  archive. It should contain a file named `pudl-jupyter.tar` -- this is 
  a Docker image which will run a Jupyter Notebook server for you locally, with
  all of the PUDL software installed and ready to use. But first you need to
  load the image into your local collection of docker images with this
  command:
  ```
  docker load -i pudl-jupyter.tar
  ```
  You should see some output at the command line as it loads the image.

* Once it's done loading, in that same directory (where you should also see a
  file named `docker-compose.yml`), run the command:
  ```
  docker-compose up
  ```

* You should see some logging messages as the PUDL Docker image starts up and
  runs the Jupyter Notebook server. Near the end of those logging message, you
  should see several possible links to click or copy-and-paste.
  Pick one that starts with `https://localhost:48512` or
  `https://127.0.0.1:48512` and open it in a web browser. This is a local web
  address -- the Jupyter Notebook server is running on your computer, not out
  on the remote internet somewhere.

* You should see JupyterLab launcher and notebook interface. In the file
  browser in the left hand sidebar, you should see a `notebooks` directory with
  several example notebooks in it, which (hopefully!) you will be able to run.

## Option 2: Request an account on our JupyterHub
We also have an experimental shared JupyterHub currently maintained in
collaboration with [2i2c.org](https://2i2c.org). Once you
have an account on our hub, you can
[work through the example notebooks there](https://catalyst-cooperative.pilot.2i2c.cloud/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fcatalyst-cooperative%2Fpudl-examples&urlpath=lab%2Ftree%2Fpudl-examples%2Fnotebooks%2F01-pudl-database.ipynb&branch=main) without needing to download anything or install
anything.

If you'd like to get an account email:
[pudl@catalyst.coop](mailto:pudl@catalyst.coop)

## Development-Oriented Usage

### Running the PUDL Jupyter Container with no data
If you just want the PUDL software environment without the processed data, for
development or other purposes, you can pull a Docker image from the
[catalystcoop/pudl-jupyter repository on DockerHub](https://hub.docker.com/r/catalystcoop/pudl-jupyter) directly:
```
docker pull catalystcoop/pudl-jupyter:latest
```
This image is built automatically using
[`repo2docker`](https://github.com/jupyterhub/repo2docker) whenever a commit
is made to the 
[pudl-examples repository](https://github.com/catalyst-cooperative/pudl-examples)

### Environment Variables
The Docker container needs to be pointed at a couple of local directories to
work properly with PUDL. These paths are set using environment variables:
* `PUDL_DATA` is the path to the PUDL directory containing your PUDL
`data`, `sqlite` and `epacems` directories. It is treated as read-only, and by
default is set to `./pudl_data`
* `USER_DATA` is a local directory that you want to have access to
within the container. It can contain other data, or your own notebooks, etc. by
default it is set to `./user_data`

You can change these defaults by editing the `.env` file in the top directory of
this repository (or the archive you downloaded from Zenodo)

To be able to fill in data using the EIA API, you'll need to [obtain an API KEY
from EPA](https://www.eia.gov/opendata/register.php). If you set an environment
variable called `API_KEY_EIA` in your shell, and run the
`catalystcoop/pudl-jupyter` container using `docker-compose` it will be passed
in to the container and available for use automatically.

## Contact Us
* Web: [Catalyst Cooperative](https://catalyst.coop)
* Email: [pudl@catalyst.coop](mailto:pudl@catalyst.coop)
* Twitter: [@CatalystCoop](https://twitter.com/CatalystCoop)
