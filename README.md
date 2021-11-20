<p align="center">
  <a href="https://aiven.io/">
    <img src="https://ftisiot.net/images/og-aiven-io-logo.png" alt="Logo" width=150 height=auto>
  </a>

  <p align="center">
    Getting started with Aiven for MySQL
    <br>
    <a href="https://github.com/aiven/devportal/issues">Report bug</a>
    ·
    <a href="https://github.com/aiven/devportal/issues">Request feature</a>
  </p>
</p>

## Table of contents

- [Getting Started](#getting-started)
- [Python Example](#python-example)
- [Sandbox project](#sandbox-project)
- [Contributors](#contributors)
- [Contributing to this document](#creators)

## Getting started

Aiven for MySQL services are managed from the [Aiven Console](https://console.aiven.io/). 

Log in to the console with your email address and password. The Console opens in the Services, where you can see all the services in the currently selected project.

To get started with Aiven for MySQL, first click the "Create a new service" button.

![image](./img/console.png)

In the dialog that opens, specify the main service properties:

- **Service name** : A short name for the service. A random name is provided, but you can type in a more human-friendly name.
- **Service type**: Select "MySQL". Currently only MySQL version 8 is supported.
- **Plan**: How many servers and what kind of memory/CPU/disk resources will be allocated to run your service. Note that the single-node "Hobbyist" and "Startup" plans are not recommended for applications where durability of the data is essential. Select one of the "high availability" plans for speedy recovery and best protection against data loss against critical node failures.
- **Cloud**: Which cloud and region to run the service on. Note that the pricing of the same service may differ between cloud providers and their regions.

After making the selections, click the "**Create**" button. 

The view returns to the service list, where the new service is shown with an indicator that it is being created.

Click the service name in the list to go to the "**Overview**" page. This view shows the connection parameters for your service and its current status. Here you can make changes to the service if necessary. 

The "**Status**" indicator says "**REBUILDING**" while the service is being created. Once the service is up and running, the light changes to green and the indicator says "**RUNNING**". 

Note that while typically services start in a couple of minutes, the performance between clouds varies and it can take longer under some circumstances.

![image](./img/new-service.png)

There are multiple ways you can try out your new MySQL service. From the command line, the easiest option might be the mysqlsh tool provided by Oracle, which can directly accept the service URL shown on the Service Overview page:

```
mysqlsh --sql mysql://avnadmin:giufg3yd1b89sqjb@rauli-mysql-dev-sandbox.aivencloud.com:12691/defaultdb?ssl-mode=REQUIRED

MySQL ssl defaultdb SQL> select 1 + 2 as three;
+-------+
| three |
+-------+
|     3 |
+-------+
1 row in set (0.0539 sec)
```

The basic `mysql` command line tool may be used as well, but you need to manually specify individual parameters. (Note: if you are giving the password via the command line, you must pass it as shown below; putting a space between the parameter name and value does not work like it does for other parameters):

```
mysql --user avnadmin --password=giufg3yd1b89sqjb --host rauli-mysql-dev-sandbox.aivencloud.com --port 12691 defaultdb
```

You can also use a graphical client like [MySQL Workbench](https://www.mysql.com/products/workbench/) to connect. Enter the individual connection parameters as shown on Service Overview page and also download the SSL CA certificate and specify the file on SSL page.

![image](./img/sql-workbench.png)

Using SSL is strongly recommended. In order to make a properly secure connection, download the CA certificate and configure it in client settings.

To create more databases, go to to the service's **Databases** tab, type in a database name in the **Create a new database** box, and click the "**Add database**" button. 

To add database users, go to the service's **Users** tab. When adding a new user you can choose the authentication method to use. By default, the web console uses the `caching_sha2_password` authentication mechanism. To successfully connect, your client libraries need to be new enough. If for any reason you are forced to use a client that only supports the older `mysql_native_password` authentication mechanism, select this separately while adding the user. (You can also later change this on the **Users** tab.) Note that changing the authentication method for a user resets the password.

## Python example
This Python example uses the [PyMySQL](https://github.com/PyMySQL/PyMySQL) library for connecting to Aiven MySQL. The host, password and port values are just examples and you should replace them with whatever is shown for your own service.

```
import pymysql

timeout = 10
connection = pymysql.connect(
    charset="utf8mb4",
    connect_timeout=timeout,
    cursorclass=pymysql.cursors.DictCursor,
    db="defaultdb",
    host="rauli-mysql-dev-sandbox.aivencloud.com",
    password="giufg3yd1b89sqjb",
    read_timeout=timeout,
    port=12691,
    ssl={"ca": "/home/rikonen/Downloads/ca.pem"},
    user="avnadmin",
    write_timeout=timeout,
)

try:
    cursor = connection.cursor()
    cursor.execute("CREATE TABLE mytest (id INTEGER PRIMARY KEY)")
    cursor.execute("INSERT INTO mytest (id) VALUES (1), (2)")
    cursor.execute("SELECT * FROM mytest")
    print(cursor.fetchall())
finally:
    connection.close()
```

## What should expect from this document

## Sandbox project 

## Contributors

## Contributing to this document

Keep calm and code on :metal: