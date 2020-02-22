---
title: "Examples"
date:
draft: false
weight: 40
---

This section provides examples of using `pg_featureserv`.

## Load Natural Earth data

### Database Preparation

The following terminal commands will create a database named `naturalearth`, assuming that your user account has create database privilege:

```
createdb naturalearth
```

Load the PostGIS extension as superuser (`postgres`):

```
psql -U postgres -d naturalearth -c 'CREATE EXTENSION postgis'
```

### Import Shapefile

The data used in the examples are loaded from [Natural Earth](https://www.naturalearthdata.com/downloads/50m-cultural-vectors/).

Download the *Admin 0 - Countries* ZIP and extract to a location on your 
machine. In that directory, run the following command in the terminal to load the 
shapefile data into the `naturalearth` database. This creates a new table `ne_50m_admin_0_countries`, with the application user as the owner -- refer to [Tables and Views](../usage/tables/) and [Security](../usage/security/) for more information on access to spatial tables on `pg_featureserv`.

```
shp2pgsql -D -s 4326 ne_50m_admin_0_countries.shp | psql -U username -d naturalearth
```

You should see the `ne_50m_admin_0_countries` table with the `\dt` SQL shell command.

Make sure that `pg_featureserv` connection specifies `naturalearth`, i.e.: `DATABASE_URL=postgres://username:password@host/naturalearth`. With the service running, you should also see the feature layer including metadata on the web interface, e.g.: http://localhost:9000/collections/public.ne_50m_admin_0_countries.html

"Features as GeoJSON" shows the GeoJSON data returned by the server, while "Features as HTML" displays an HTML preview. 

The interface returns 10 features by default. To display more features in the HTML preview (e.g. to display all countries in this example), select "500" in the Limit dropdown, and click "Query."

![pg_featureserv HTML preview](/example-web-preview-html.PNG)


