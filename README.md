<div align="center" >
    <img width="350" height="150" src="frontend/public/logo.png">
</div>

[![Github License](https://img.shields.io/github/license/Headstorm/shukra)](https://github.com/Headstorm/shukra/blob/master/LICENSE)
[![Github Issues](https://img.shields.io/github/issues/Headstorm/shukra)](https://github.com/Headstorm/shukra/issues)
[![Github Forks](https://img.shields.io/github/forks/Headstorm/shukra)](https://github.com/Headstorm/shukra/)
[![Github Stars](https://img.shields.io/github/stars/Headstorm/shukra)](https://github.com/Headstorm/shukra/)

# Shukra #

Shukra is an Akka cluster visualization and management dashboard for inspecting local and remote Akka clusters. 

Shukra is lovingly developed by [Headstorm's](https://www.headstorm.com/) Open Source group. Please reach out to us at: opensource@headstorm.com

Shukra relies on [akka-management](https://doc.akka.io/docs/akka-management/current/akka-management.html) which exposes an HTTP interface to interact with an Akka cluster.

![image](https://user-images.githubusercontent.com/915955/78514456-0eb86300-7777-11ea-85df-b6bdd4563fd7.png)

## Production Usage Instructions

These instructions guide you through the process to run Shukra in a production environment.

### Generate Production Build

* Set `homepage` property in `package.json` file to reflect the deployment URL of the server. For example, `"homepage": "http://localhost:8080/shukra"`. 
* Use the command `npm run build` (uses create react app `react-scripts build` internally) to generate a production build to serve it with a static server.
* The `build` folder contains files that are ready to be deployed. 
* Refer to https://create-react-app.dev/docs/production-build or https://reactjs.org/docs/optimizing-performance.html for more information on generating production ready builds.

### Deploy Production Build

* Move the files created in the `build` folder to a static server. Do not change the folder structure.
* Follow instructions in the **Configuration** section below to modify the Akka Management URL.

## Development Usage Instructions

The following instructions will get the project up and running on your local machine for development and testing purposes.

### Prerequisites

Download and install the latest version of [Vagrant](https://www.vagrantup.com/downloads.html) to spin up a sample Akka cluster (Shukra Backend). 
Download and install the latest version of [Node.js and NPM](https://nodejs.org/en/download/) to install and start the Shukra UI.

### Installation

#### Back End:

Use the commands below to start a sample Akka Docker cluster.

Potential issue #1:
"A Vagrant environment or target machine is required to run this command." Run ```vagrant init``` to create a new Vagrant environment.

Potential issue #2:
"No usable default provider could be found for your system. Vagrant relies on interactions with 3rd party systems, known as "providers", to provide Vagrant with resources to run development environments. Examples are VirtualBox, VMware, Hyper-V. The easiest solution to this message is to install VirtualBox, which is available for free on all major platforms."
Refer to [Vagrant](https://www.vagrantup.com/docs/virtualbox/) documentation to see which VirtualBox versions are compatible. Download a compatible version and Vagrant will take care of the rest!

You can now ```cd``` to the shukra project folder and run ```vagrant up```. If executed successfully, ```vagrant up``` starts Akka Management on the seed node with the endpoint `http://localhost:8402/ShukraCluster`, unless changed in the ```application.conf``` file. ```vagrant up``` will create 3 nodes, a seed and two regular nodes, called seed, c1, and c2 respectively (unless changed in ```docker-compose.yml```). If the front-end is running, you should be able to see these nodes appear in the UI upon refreshing the homepage.

You can now use [docker-compose](https://docs.docker.com/compose/reference/ps/) commands to make the cluster nodes respond. First, open a VirtualBox terminal with this command: ```vagrant ssh```. Within the VirtualBox terminal, ```cd /vagrant```, and then type some commands! Some easy commands to start with are:
```docker-compose stop c1``` - Stops running containers without removing them.
```docker-compose start c1```- Starts existing containers for a service.
```docker-compose ps``` Lists containers.
```docker-compose logs c1``` - Displays log output from services.

#### Front End:

Navigate to the `frontend` folder and use the command below to install the dependencies for Shukra UI.
```
npm install
```

Use the command below to start the Shukra UI.
```
npm start
```

### Configuration

#### Back end:

Akka Management for a member is disabled by default.

In order to enable Akka Management for a member, set `AKKA_MANAGEMENT_ENABLE: enabled` property under `environment` section in `docker-compose.yml`.

Specify the `AKKA_MANAGEMENT_PORT: <port-number>` property to change the Akka Management HTTP endpoint port.

Properties `SEED_PORT_1600_TCP_ADDR` and `SEED_PORT_1600_TCP_PORT` can be used to modify the seed node for a member.

#### Front end:

By default, Shukra UI points to the Akka HTTP URL `http://localhost:8402/ShukraCluster`.

For local development or testing, change the `proxy` property in `package.json` to the Akka Management Host HTTP server address and change the `akka.management.url` property in `public/akkaClusterProps.json` to the Akka Management Host HTTP base path. The `proxy` property is used to set up reverse proxy to avoid CORS issue on local environments. 

For production, change the `akka.management.url` property in `public/akkaClusterProps.json` to the entire Akka Management URL, i.e., Akka Management HTTP address + base path.

## Built With

* [Scala](https://docs.scala-lang.org/?_ga=2.243112642.1950037817.1572011844-746476698.1572011844) - Back end programming language
* [SBT](https://www.scala-sbt.org/1.x/docs/) - Back end build tool
* [Akka](https://akka.io/docs/) - Actor system framework
* [React](https://akka.io/docs/) - Front end JavaScript library

## Contributing

1. Fork it (<https://github.com/Headstorm/shukra/fork>)
2. Create your feature branch (`git checkout -b feature/fooBar`)
3. Commit your changes (`git commit -am 'Add some fooBar'`)
4. Push to the branch (`git push origin feature/fooBar`)
5. Create a new Pull Request

## Authors

* **Karthik Pasagada** - [kpasagada](https://github.com/kpasagada)
* **Zac Romick** - [zromick](https://github.com/zromick)

See also the list of [contributors](https://github.com/Headstorm/shukra/graphs/contributors) who participated in this project.

## Acknowledgments

* Thanks to [akka-docker-cluster-example](https://github.com/akka/akka-sample-cluster-docker-compose-scala) for initial akka cluster setup.

## More Information

https://github.com/Headstorm/shukra/wiki/Shukra

Shukra is the Indian god of light and clarity, and this tool is designed to shed light and clarity on previously hard to observe Akka Clusters. We can now understand what our Akka Clusters look like when they're running in production instead of guesswork from reading logs.

## Meta

Distributed under the Apache 2 license. See [LICENSE](LICENSE) for more information.
