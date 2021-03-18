# graphana-influxdb-reports-setup
# Pull and start the influxDB docker image
`docker pull influxdb:v1.8`
# Download influx config 
Download from [https://openplant.b-cdn.net/wp-content/uploads/influxdb.conf](https://openplant.b-cdn.net/wp-content/uploads/influxdb.conf) and save at location: `C:\ProgramData\InfluxDB`

`docker run -p 8086:8086 -v C:/ProgramData/InfluxDB:/var/lib/influxdb influxdb -config /var/lib/influxdb/influxdb.conf
docker exec -it influxdb bash`

# Create the webpagetest database
`CREATE DATABASE webpagetest`

# Access influxdb instance by visiting [http://localhost:8086/](http://localhost:8086/)

# Pull and run the graphana docker image 
`docker run -d -p 3000:3000 grafana/grafana`

# Add InfluxDB Datasource
* Access graphana by visiting [http://localhost:3000/](http://localhost:3000/)
* Open Settings/Add Datasource InfluxDB
* Fill in DataSource name, url: http://localhost:8086/, Select type "Browser"
* Click on Save and Test

# WebPageTest integration [https://github.com/n1xan/webpagetest-nodejs-runner](https://github.com/n1xan/webpagetest-nodejs-runner)
# Google Page Speed integration [https://github.com/n1xan/psi-report](https://github.com/n1xan/psi-report)
# JMeter integration [https://jmeter.apache.org/usermanual/realtime-results.html](https://jmeter.apache.org/usermanual/realtime-results.html)

# InfluxDb 2.x setup:
`docker pull quay.io/influxdb/influxdb:v2.0.4`
* With InfluxDB running, visit localhost:8086.
* Start image with port 8086
* Click Get Started
* Set up your initial user
* Enter a Username for your initial user.
* Enter a Password and Confirm Password for your user.
* Enter your initial Organization Name.
* Enter your initial Bucket Name.
* Click Continue.
* https://docs.influxdata.com/influxdb/v2.0/get-started/#set-up-influxdb
