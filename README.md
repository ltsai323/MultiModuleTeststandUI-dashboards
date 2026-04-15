# Grafana dashboard for monitoring PLC and stocks

Use **docker compose** for building grafana dashboard.


```
.
├── README.md
├── setup.openfirewall.sh
├── docker-compose.yml
├── dashboards
│   ├── IVcurve
│   │   ├── IV_CurveforMMTS.json
│   │   ├── IV_CurvePlot.json
│   │   └── SingleModuleIVcurve.json
│   ├── stocks
│   │   ├── HGCalAssemblyandTestStatus.json
│   │   ├── NTULab Material Stocks for HD.json
│   │   └── NTULab Material Stocks for LD.json
│   └── thermal_cycle
│       ├── ThermalBoxEnvironmentforMMTS.json
│       ├── ThermalBoxEnvironmentHistory.json
│       └── ThermalBoxEnvironment.json
├── provisioning
│   ├── alerting
│   ├── dashboards
│   │   └── dashboards.yaml
│   ├── datasources
│   │   └── datasources.yaml
│   └── plugins
└── _stable_dashboards
    ├── IVcurve
    │   ├── IV_CurveforMMTS.json
    │   ├── IV_CurvePlot.json
    │   └── SingleModuleIVcurve.json
    ├── stocks
    │   ├── HGCalAssemblyandTestStatus.json
    │   ├── NTULab Material Stocks for HD.json
    │   └── NTULab Material Stocks for LD.json
    └── thermal_cycle
        ├── ThermalBoxEnvironmentforMMTS.json
        ├── ThermalBoxEnvironmentHistory.json
        └── ThermalBoxEnvironment.json
```


### How to use
Use docker compose to handle the building. Before activate the docker container, you need to modify the following setups.

1. Assign database
Modify [provisioning/datasources/datasources.yaml](provisioning/datasources/datasources.yaml). By default you need to assign the database **hgcdb** and **PLC variables** for showing environment variables
(Once the temperature / humidity recorded into DB, than we can merge them)
```
datasources:
  - name: mac-postgres-db
    uid: mac_postgres_db
    type: grafana-postgresql-datasource
    url: 192.168.0.0:5432 ## here is the IP address to hgcdb and port 5432
    database: hgcdb
    user: hgcdbUSERname  # Update if your username is different
    secureJsonData:
      password: hgcdbPASSWORD
    jsonData:
      sslmode: 'disable'
    isDefault: true
```

Currently I used 2 data source **mac-postgres-db** and **grafana-postgresql-datasource-PLC**.
The **mac-postgres-db** is CMU postgres database **hgcdb**.
And **grafana-postgresql-datasource-PLC** is a developing database that stores PLC variable.
This data source would be removed later once the PLC variables uploaded into **hgcdb**.

So currently **thermal box** related dashboards cannot be accessed in dashboard.


2. Docker settings in `docker-compose.yml`
You need to modify the environment variables like **GF_SECURITY_ADMIN_USER** and **GF_SECURITY_ADMIN_PASSWORD** for this dashbaord.
dashboard modification reuqired the this username and password.


3. Use docker build the dashboard
After that, you can use docker compose to handle the grafana dashboard
```
sudo docker compose up -d ### compile and activate the required docker containers
sudo docker compose down  ### disable the docker containers
```

4. Check the grafana dashboard can be accessed locally
Use [local link](http://127.0.0.1:3000/) checks dashboard works or not.

5. Open firewall
```
sudo sh setup.openfirewall.sh
```
open port 3000. For raspberry pi, you need to install `firewall-cmd`.


### To do list
- [ ] How do I make grafana dashboard using https instead of http

