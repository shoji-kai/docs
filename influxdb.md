# influxdb

## インストール

```
% curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -
% sudo vim /etc/apt/sources.list.d/influxdb.list
deb https://repos.influxdata.com/ubuntu xenial stable
% sudo apt update
% sudo apt install -y influxdb
% sudo systemctl start influxdb
```


## Getting Started

### ログイン

```
% influx -precision rfc3339
Connected to http://localhost:8086 version 1.4.3
InfluxDB shell version: 1.4.3
>
```

### CREATE DATABASE <DBNAME>

```
> create database mydb
```

### SHOW DATABASES

```
> show databases
name: databases
name
----
_internal
mydb
```

### USE <DBNAME>

```
> use mydb
Using database mydb
```

### INSERT

書式:
INSERT <measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]

```
> INSERT cpu,host=serverA,region=us_west value=0.64
> INSERT payment,device=mobile,product=Notepad,method=credit billed=33,licenses=3i 1434067467100293230
> INSERT stock,symbol=AAPL bid=127.46,ask=127.48
> INSERT temperature,machine=unit42,type=assembly external=25,internal=37 1434067467000000000
```

## HTTP API 

```
% curl -i -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"
```

```
% curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
```

```
% curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server02 value=0.67
cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257'
```

```
% cat cpu_data.txt
cpu_load_short,host=server02 value=0.67
cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257
% curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary @cpu_data.txt
```

```
% curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=mydb" --data-urlencode "q=SELECT \"value\" FROM \"cpu_load_short\" WHERE \"region\"='us-west'"
```


