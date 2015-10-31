# Swagger + bitjws Example App

The app is a Coin collection, where Users identified by bitjws keys can create and manage virtual Coins stored on the server. For detailed documentation of the API, see the [swagger spec](http://deginner.github.io/swaxample-ui/).


##### Deginner Application Stack

| Project | Maintainer |    Version    |  Description                    | Swagxample Use |
|---------|------------|---------------|---------------------------------|----------------|
|[Flask](http://flask.pocoo.org/) | [Flask](http://flask.pocoo.org/) | [![PyPi version](https://img.shields.io/pypi/v/flask.svg)](https://pypi.python.org/pypi/flask/) | Flask is a microframework for Python based on Werkzeug, Jinja 2 and good intentions. | Flask is the web server framework used. |
|[Swagger](http://swagger.io/) | [Swagger](http://swagger.io/) | 2.0 | Swagger is a simple yet powerful representation of your RESTful API. | Swagxample includes a Swagger spec which is used to configure the [bravado-bitjws](https://github.com/deginner/bravado-bitjws) client automatically. |
|[SQLAlchemy](https://sqlalchemy.org) | [SQLAlchemy](https://sqlalchemy.org) | [![PyPi version](https://img.shields.io/pypi/v/sqlalchemy.svg)](https://pypi.python.org/pypi/sqlalchemy/) | SQLAlchemy is the Python SQL toolkit and Object Relational Mapper that gives application developers the full power and flexibility of SQL. | SQLAlchemy is used as a generic and configurable DB interface. |
|[bitjws](https://github.com/deginner/bitjws) | [Deginner](https://github.com/deginner) | [![PyPi version](https://img.shields.io/pypi/v/bitjws.svg)](https://pypi.python.org/pypi/bitjws/) |JWS ([JSON Web Signature](http://self-issued.info/docs/draft-ietf-jose-json-web-signature.html)) using Bitcoin message signing as the algorithm.| Signing and message serialization. |
|[flask-bitjws](https://github.com/deginner/flask-bitjws) | [Deginner](https://github.com/deginner) | [![PyPi version](https://img.shields.io/pypi/v/flask-bitjws.svg)](https://pypi.python.org/pypi/flask-bitjws/) |[Flask](http://flask.pocoo.org) extension for [bitjws](https://github.com/g-p-g/bitjws) authentication. | Manage bitjws integration for Flask server. |
|[flask-swagger](https://github.com/deginner/flask-swagger) | [gangverk](https://github.com/gangverk) | [![PyPi version](https://img.shields.io/pypi/v/flask-swagger.svg)](https://pypi.python.org/pypi/flask-swagger/) |A Swagger 2.0 spec extractor for Flask. | Extract path info from flask app and update swagger spec. |
|[bravado-bitjws](https://github.com/deginner/bravado-bitjws) | [Deginner](https://github.com/deginner) | [![PyPi version](https://img.shields.io/pypi/v/bravado-bitjws.svg)](https://pypi.python.org/pypi/bravado-bitjws/) |Bravado-bitjws is an add on for [Bravado](https://github.com/Yelp/bravado) that allows [bitjws](https://github.com/g-p-g/bitjws) authentication.| Client for the application, used in tests. |
|[sqlalchemy-login-models](https://github.com/deginner/sqlalchemy-login-models) | [Deginner](https://github.com/deginner) | 0.0.4 | User related data models for a server using [SQLAlchemy](http://www.sqlalchemy.org/), and [json schemas](http://json-schema.org/). | User and UserKey models provide basis for app login management. |
|[Deglet](https://bitbucket.org/deginner/deglet) | [Deginner](https://github.com/deginner) | 0.0.1 | A multisignature Bitcoin client based on [React](https://facebook.github.io/react/) and [Bitcore](http://bitcore.io). | Not used in Swagxample yet. |


## Usage

Start the server in debugging mode.

`python swagxample/server.py`

Start the server using gunicorn, as suitable for a production environment.

`make run`

## Automated Swagger Updates

To update the swagger spec's paths, flask-swagger provides a generator. This can be run with `make swagger`, but it is worth looking at what is happening.

`flaskswagger swagxample.server:app --template swagxample/static/swagger.json --out-dir swagxample/static/`

This crawls the app's routes looking for flask-swagger docstrings. If so, it updates the template and outputs it. In this case, the spec is being edited in place. The net result is that the spec's paths will be updated based on the latest docstrings in your app.

The definitions in this example were also automatically generated, those using [alchemyjsonschema](https://github.com/podhmo/alchemyjsonschema). It's command schema extractor was run on both [sqlalchemy-login-models](https://github.com/deginner/sqlalchemy-login-models) and the SQLAlchemy model(s) in this repo. For example (where $SWAGXAMPLE_APP_HOME is the root of this repo):

`alchemyjsonschema sqlalchemy_login_models.model -s --out-dir $SWAGXAMPLE_APP_HOME/swagxample/static`

## Configuration

This app expects a Python configuration file, which can be specified using the SWAGXAMPLE_CONFIG_FILE environmental variable.

`export SWAGXAMPLE_CONFIG_FILE = /path/to/cfg.py`

The format of the config file is as shown in example_cfg.py. Be sure to change the keys before deploying in production!

## Tests

By exercizing all of the Deginner components in an integrated context, functional completion can be tested, along with complex use cases. For this reason, this example app has more unit tests than strictly necessary, and include use of the relatively heavy (for unit tests) [bravado-bitjws](https://github.com/deginner/bravado-bitjws) client.

Currently the tests expect the server to be running on 0.0.0.0:8002, like the default `make run` behavior. This test method is scheduled for an immediate upgrade.

`python setup.py pytest`
