# Notes API
A simple note taking api with CRUD operations

### Prerequisites

  - Apache Maven 3+
  - [Martini Desktop](https://www.torocloud.com/martini/download)

### Building the Martini Package

```
$ mvn clean package
```
This will create a ZIP file named `notes-api.zip` containing all the files (services, configurations, etc.) needed under the `target` folder. This ZIP file is what we call a [Martini Package](https://docs.torocloud.com/martini/latest/developing/package/) which then you can import in Martini Desktop to get started. You can learn more how to import a Martini Package by visiting our [documentation](https://docs.torocloud.com/martini/latest/developing/package/importing/)

### Getting the Martini Package from TORO Marketplace

You can also get this package via TORO Marketplace. See our documentation on [How to import a Martini Package from Marketplace](https://docs.torocloud.com/martini/latest/developing/package/importing/#from-the-marketplace).

### Usage
This package exposes operations for a simple notes REST API that allows you to do CRUD operations. You can find the [Gloop REST API](https://docs.torocloud.com/martini/latest/developing/gloop/api/rest/) file at `/api/Notes` under the `code` folder after importing the package to your Martini Desktop application.

### Setup
This Martini Package automatically sets up the required resources for you. During the package startup, Martini will execute the configured _Startup Service_ that initializes the database to be used for storing the record that will be creating for using this notes api.

This package uses HSQL as the default datastore but you can change this to MySQL, Postgres and etc by updating the [package properties](https://docs.torocloud.com/martini/latest/developing/package/properties/) located at [conf](https://docs.torocloud.com/martini/latest/developing/package/directory/) folder. Below is what the contents of the properties file look like

```
# The database connection name to be used
database.name=notes

# The database type to be used
database.type=hsql

# HSQL database configuration to be used
hsql.driver=org.hsqldb.jdbc.JDBCDriver
hsql.connection.url=jdbc:hsqldb:file:${toroesb.home}data/hsql/notes.db;hsqldb.tx=MVCC;sql.syntax_mys=true
hsql.username=sa
hsql.password=

# MySQL database configuration to be used
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.connection.url=jdbc:mysql://localhost:3306/notes?allowMultiQueries=true&sessionVariables=sql_mode='ANSI'
mysql.username=root
mysql.password=
```
* `database.type` sets the database provider the Martini package will use. If you will use the default hsql config, you don't need to change anything in the hsql configuration. **Note**: If you will use a different hsql database, make sure that you add `sql.syntax_mys=true` in the connection properties. This ensures that the SQL query from the SQL Services in this package will be compatible with hsql.

### Operations

The base url is `<host>/api/notes/1.0` where `host` is the location where the Martini is deployed. By default, it's `localhost:8080`.

#### Notes

`GET /notes`

Returns a list of all notes

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/notes/1.0/notes \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all note",
    "warnings": [],
    "note": [
        {
            "note_id": "02f89510-3ace-4a87-84e8-6f46b6644bd8",
            "subject": "The Red Fox",
            "body": "The quick brown fox jump over the fence",
            "created_at": "2020-03-26T11:38:00+0800",
            "updated_at": "2020-03-26T11:38:00+0800"
        },
        ...
        {
            "note_id": "715a9d18-c84b-4cc9-b077-bbfa41c68040",
            "subject": "The Plant",
            "body": "The plant grows happily and nicely",
            "created_at": "2020-03-26T11:42:36+0800",
            "updated_at": "2020-03-26T11:42:36+0800"
        }
    ]
}
```

`POST /notes`

Creates a new note

**Sample Request**

**curl**
```
curl -X POST \
  http://localhost:8080/api/notes/1.0/notes \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "subject": "The Red Fox",
    "body": "The quick brown fox jump over the fence"
}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully create note",
    "warnings": [],
    "note": {
        "note_id": "02f89510-3ace-4a87-84e8-6f46b6644bd8",
        "subject": "The Red Fox",
        "body": "The quick brown fox jump over the fence",
        "created_at": "2020-03-26T11:38:00+0800",
        "updated_at": "2020-03-26T11:38:00+0800"
    }
}
```

`PUT /notes`

Updates a note

**Sample Request**

**curl**
```
curl -X POST \
  http://localhost:8080/api/notes/1.0/notes \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "note_id": "715a9d18-c84b-4cc9-b077-bbfa41c68040",
    "subject": "The Big Plant"
}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully update note",
    "warnings": []
}
```

`GET /notes/<note_id>`

Returns a single note that matches the given `note_id`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/notes/1.0/notes/02f89510-3ace-4a87-84e8-6f46b6644bd8
```

**Sample Response**

If the request is sucessful, it will return an HTTP response `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get note",
    "warnings": [],
    "note": {
        "note_id": "02f89510-3ace-4a87-84e8-6f46b6644bd8",
        "subject": "The Red Fox",
        "body": "The quick brown fox jump over the fence",
        "created_at": "2020-03-26T11:38:00+0800",
        "updated_at": "2020-03-26T11:38:00+0800"
    }
}
```

`DELETE /notes/<note_id>`

Deletes an note that matches the given `note_id`

**Sample Request**

**curl**
```
curl -X DELETE \
  http://localhost:8080/api/notes/1.0/notes/715a9d18-c84b-4cc9-b077-bbfa41c68040
```

**Sample Response**

If the request is sucessful, it will return an HTTP response `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully delete note",
    "warnings": []
}
```