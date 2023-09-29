# DreamDevBox tests

**Important:** Do not run this locally.

The Makefile will
* overwrite your `.env` file.
* empty your local DreamDevBox emails
* empty your local DreamDevBox logs


## Test workflow

If you nevertheless want to run the tests locally, follow the instructions below in that order.

#### Initialization
```bash
# Overwrites .env with testing defaults
make init
```

#### Configure
```bash
# Configure custom versions (optionally)
make configure SRV=PHP_SERVER VER=5.6
make configure SRV=HTTPD_SERVER VER=apache-2.2
```

#### Start
```bash
make start
```

#### Info
```bash
make info
```

#### Test

```bash
make test
```

#### Stop
```bash
make stop
```
