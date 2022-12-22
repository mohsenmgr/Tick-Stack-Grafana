# Tick-Stack-Grafana
This is a complete Tick Stack configured in Docker, Contains Telegraf, InfluxDB, Chronograf and Kapacitor. Kapacitor Does not work Unfortunately.

## Project Setup

Telegraf enables you to monitor your System's information by enabling an extension inside InfluxDB. 

### Telegraf setup is as follows:
1.Run 

```sh
docker-compose up
```

2.Go on the browser and head to http://localhost:8086/
Here you can set admin, admin password, organization Name and Initial (default) Bucket.
I used
User: admin
Password: admin1234
Organization: Tera
Initial Bucket: my-initial-bucket

3.You will see Getting Started Page. Click Load Your Data and Search for System in the Search Box. Click on the circular gauge icon and then click Use this plugin.
Name your Configuration and then select the inital Bucket that was created in 2nd step, finally click continiue configuring.
After specifying Description Click Save and Test button.
You need to Copy the String which comes after export INFLUX_TOKEN and put it inside the telegraf.conf token field which is located in [[outputs.influxdb_v2]] section of the configuration file. Make sure necessary fields are uncommented as the file we have in this repository.
Click Finish.

4.On the same page there should be a Tab called API TOKENS at the far right of the top nav bar. Click on it!
You will see two API Tokens (first one is admins token used later for Grafana Setup) and the second one is my-initial-bucket read and write token. 
If you need to generate another token to access your initial-bucket you can clone your initial-bucket token and generate a new token to use inside telegraf.conf

5.Stop Telegraf from Docker and Start again. This time it should run fine and send System information to the influxDB Bucket. You can test this by creating a simple dashboard inside InfluxDB and see your realtime data (timeseries)

### Grafana setup is as follows:

Head to http://localhost:3000/login. You can login to grafana by using 
Username: admin
Password: admin
and then you can set the New Password. (obviously admin admin again because passwords suck!)

Click on Data Sources Tile, Select influxDB, choose Flux in the Query language combobox, apparantly InfluxQL does not work with the new versions of influxDB.

URL should be the container containing the influxdb, http://influxdb:8086
Do not touch the radio buttons(all off)
Head to InfluxDB Details and add Organization, (mine is Tera)
Remember in influxDB API Token we had Admins token ? Yeah we need to clone that, so head to the influxDB local website http://localhost:8086/ and go to API Tokens, admins token and clone that sucker then copy paste it into the field.
Add your default bucket (mine was my-initial-bucket) and Finally Click Save & Test.
If you did everything right and the gods of settings are on your side (very boring gods) you shall have connection to your influxDB database buckets within the grafana.

I wrote this because I had to search many pages for a solution for this garbage stack ! honestly why no piece of software work seamlessly with another one. Well i hope it was helpful otherwise we have wasted our lives on this ...t.


Note: if Telegraf Still does not connect to the InfluxDB try to clone the Initial-bucket API Token once again and put the new Token inside the telegraf.conf file token field. It should work.

Bye
    












