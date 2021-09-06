# Performance & Load Testing Infrastructure Setup

The following file will present instructions for complete Performance Testing infrastucture setup on your local machine. The infrastructure contains the following tools and integrations:

Performance Testing Tools:
* Local WebPageTest instance
* Google PageSpeed Insights

Load Testing Tool:
* JMeter 

Database storage:
* InfluxDB

Reporting and monitoring:
* Grafana
* Telegraf

## Prerequisites
* Git installed - [link](https://git-scm.com/downloads)
* Code Editor (Visual Studio Code is preffered) - [link](https://code.visualstudio.com)
* Docker capable of executing Linux containers - [link](https://www.docker.com/products/docker-desktop)
* Node.js installed globally - [link](https://nodejs.org/en/download/)
* [Optional] Homebrew for macOS users - [link](https://brew.sh)


## InfluxDB setup
1. Pull and start the InfluxDB docker image in Terminal
    
   *Windows/macOS/Linux*
   ```bash
   docker run -d -p 8086:8086 --name influxdb influxdb:1.8
   ```
   *Windows/macOS/Linux*
   ```bash
   docker exec -it influxdb bash
   ```
1. Create the databases needed for all tools
    *Windows/macOS/Linux*
    ```bash
    influx
    ```
    *Windows/macOS/Linux*
    ```bash
    CREATE DATABASE webpagetest
    CREATE DATABASE googlepagespeed
    CREATE DATABASE telegraf
    CREATE DATABASE jmeter
    ```

1. Access InfluxDB instance

    🌐Navigate to [http://localhost:8086/](http://localhost:8086/) - 404 error IS EXPECTED

    🌐Navigate to [http://localhost:8086/query?pretty=true&q=SELECT%20*%20FROM%20%22webpagetest%22&db=webpagetest](http://localhost:8086/query?pretty=true&q=SELECT%20*%20FROM%20%22webpagetest%22&db=webpagetest) to query the newly created Database

## Grafana setup
1.Pull and run the grafana docker image 

```
docker run -d -p 3000:3000 --name grafana grafana/grafana
```

1. Add InfluxDB Datasource
* 🌐Access graphana by visiting [http://localhost:3000/](http://localhost:3000/)
* Login using admin/admin
* Set new password
* Open Settings/Add Datasource InfluxDB
* Fill in:
   - DataSource name: **InfluxDB**
   - URL: **http://localhost:8086**
   - Access: Select type **"Browser"**
   - Database name: **webpagetest**
* Click on Save & Test
* Expected result:

![Screen Shot 2021-09-02 at 17 00 09](https://user-images.githubusercontent.com/1863261/131857421-1d194854-918d-491d-abb8-9885a8e9fa84.png)

## WebPageTest setup ([repo link](https://github.com/n1xan/webpagetest-docker-setup))

1. Clone repo
    ```git
    git clone https://github.com/n1xan/webpagetest-docker-setup.git
    cd webpagetest-docker-setup
    ```
2. Build docker images


*Windows*

    .\build-docker-images.bat
    
*macOS/Linux*

    ./build-docker-images.sh

3. Start server and agent images

*Windows*

    .\start-server-and-agent.bat
*macOS/Linux*

    ./start-server-and-agent.sh
4. Access WebPageTest instance

* 🌐Navigate to http://localhost:4000/
* ✔Start a new test with Location *Test Location*


![MicrosoftTeams-image (2)](https://user-images.githubusercontent.com/1863261/131881084-3ed6a8cc-f73a-42db-bc17-b22b89d7e858.png)


## Run WebPageTest tests with InfluxDB logging [repo link](https://github.com/n1xan/webpagetest-nodejs-runner)
1. Clone repo
    ```git
    git clone https://github.com/n1xan/webpagetest-nodejs-runner.git
    cd webpagetest-nodejs-runner
    ```
2. Install dependancies and start a test
    ```git
    npm install
    node webpagetest.js "https://example.com" example.com LAN Chrome
    ```
![MicrosoftTeams-image (3)](https://user-images.githubusercontent.com/1863261/131882287-0e0bc177-7e7f-4f67-827b-b142ba4cd87a.png)

## Run Google PageSpeed test with InfluxDB Logging  [repo link](https://github.com/n1xan/psi-report)
1. Clone repo
    ```git
    git clone https://github.com/n1xan/psi-report.git
    cd psi-report
    ```
2. Install dependancies and start a test
    ```git
    npm install
    node run run.js https://example.com
    ```
![MicrosoftTeams-image (5)](https://user-images.githubusercontent.com/1863261/131882686-707b5ab2-f960-484d-a7b2-a3b28b484143.png)


## Run JMeter test with InfluxDB integration
1. Follow the tutorial: [User manual link](https://jmeter.apache.org/usermanual/realtime-results.html#influxdb_db_configuration)
2. Load sample test
    ```git
    git clone https://github.com/n1xan/jmeter-influx-integration.git
    cd jmeter-influx-integration
    jmeter -t DemoBellatrixNavigation.jmx
    ```

## Configure JMeter test Grafana visualization
1. Follow the tutorial: [User manual link](https://jmeter.apache.org/usermanual/realtime-results.html#grafana_configuration)

## Telegraf Server monitoring

1. Download Telegraf for *Windows/macOS/Linux*: [Download Link](https://portal.influxdata.com/downloads/#app-telegraf)

2. Run telegraf application
*Windows*
- Unzip
- Edit telegraf.config
  - Set database name /should be created/
- Save
- cmd `telegraf.exe --config telegraf.conf`
- debug `telegraf.exe --config telegraf.conf --debug`

*macOS*
1. Download config in local folder
    ```
    mkdir ~/.telegraf
    cd ~/.telegraf
    curl -O https://raw.githubusercontent.com/influxdata/telegraf/master/etc/telegraf.conf
    ```
2. Edit telegraf.config
  - `open ~/.telegraf`
  - Uncomment [row 119](https://github.com/influxdata/telegraf/blob/master/etc/telegraf.conf#L119)
  - Set your database name if different than `telegraf`
  - Save file
3. Start Telegraf application
