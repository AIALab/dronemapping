<img alt="WebODM" src="https://github.com/AIALab/dronemapping/blob/master/Screenshot%20from%202018-04-27%2017-10-56.png" width="180">

A user-friendly, extendable application and [API] for drone image processing. Generate georeferenced maps, point clouds, elevation models and textured 3D models from aerial images. It uses OpenDroneMap for processing.

* [Getting Started](#getting-started)

![Alt text](https://github.com/AIALab/dronemapping/blob/master/screenshots/Screenshot%20from%202018-05-12%2000-21-08.png)

![Alt text](https://github.com/AIALab/dronemapping/blob/master/screenshots/Screenshot%20from%202018-05-12%2000-21-26.png)



## Getting Started

* Install the following applications (if they are not installed already):
 - [Docker](https://www.docker.com/)
 - [Python](https://www.python.org/downloads/)
 - [Pip](https://pypi.python.org/pypi/pip/)
 - [Git](https://git-scm.com/downloads)

* Windows users have a choice between Docker Toolbox (older product but more tutorials available) and Docker for Windows (more recent version that runs on Microsoft's Hyper-V virtualization engine, recommended by Docker). Docker for Windows users should set up their Docker environment before launching WebODM using the Docker utility in the system tray: 1) make sure Linux containers are enabled (Switch to Linux Containers...), 2) give Docker enough CPUs (default 2) and RAM (>4Gb, 16Gb better but leave some for Windows) by going to Settings -- Advanced, and 3) select where on your hard drive you want virtual hard drives to reside (Settings -- Advanced -- Images & Volumes) . 

* From the Docker Quickstart Terminal or Powershell (Windows), or from the command line (Mac / Linux), type:
```bash
git clone https://github.com/AIALab/dronemapping --config core.autocrlf=input
cd dronemapping
./webodm.sh start
```

* Open a Web Browser to `http://localhost:8000` (unless you are on Windows using Docker Toolbox, see below)

Docker Toolbox users need to find the IP of their docker machine by running this command from the Docker Quickstart Terminal:

```bash
docker-machine ip
192.168.1.100 (your output will be different)
```

The address to connect to would then be: `http://192.168.1.100:8000`.

To stop WebODM press CTRL+C or run:

```
./webodm.sh stop
```
We recommend that you read the [Docker Documentation](https://docs.docker.com/) to familiarize with the application lifecycle, setup and teardown, or for more advanced uses. Look at the contents of the webodm.sh script to understand what commands are used to launch WebODM.


### Backup and Restore

If you want to move WebODM to another system, you just need to transfer the docker volumes (unless you are storing your files on the file system).

On the old system:

```bash
mkdir -v backup
docker run --rm --volume webodm_dbdata:/temp --volume `pwd`/backup:/backup ubuntu tar cvf /backup/dbdata.tar /temp
docker run --rm --volume webodm_appmedia:/temp --volume `pwd`/backup:/backup ubuntu tar cvf /backup/appmedia.tar /temp
```

Your backup files will be stored in the newly created `backup` directory. Transfer the `backup` directory to the new system, then on the new system:

```bash
ls backup # --> appmedia.tar  dbdata.tar
./webodm.sh start && ./webodm.sh down # Create volumes
docker run --rm --volume webodm_dbdata:/temp --volume `pwd`/backup:/backup ubuntu bash -c "rm -fr /temp/* && tar xvf /backup/dbdata.tar"
docker run --rm --volume webodm_appmedia:/temp --volume `pwd`/backup:/backup ubuntu bash -c "rm -fr /temp/* && tar xvf /backup/appmedia.tar"
./webodm.sh start
```
