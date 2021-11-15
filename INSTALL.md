# Preliminaries for Ubuntu 16.04

Installation for other distros may be slightly different. This assumes NodeJS 8.3 or higher and npm 5.6 are already installed. All of this is done in the lamassu-server directory.

## Packages

```
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib postgresql-server-dev-9.5 libpq-dev git
```

## Set up PostgreSQL

```
sudo -u postgres createdb lamassu
sudo -u postgres psql postgres
```

In ``psql``, run the following and set password to ``postgres123``:

```
\password postgres
ctrl-d
```

# Preliminaries for MacOS

## Postgres

Use Postgres.app: https://postgresapp.com/

**psql** is automatically installed. You won't need to set up users.

## NodeJS

```
curl -L https://git.io/n-install | bash -s -- -y lts
. ~/.bash_profile
```

# Installation

## Install node modules

Make sure you're running NodeJS 8.3 or higher. Ignore any warnings.

```
npm install
```

## Generate certificates

```
bash tools/cert-gen.sh
```

Notes: 
  - This will create a ``.lamassu`` directory in your home directory.
  
## Set up database

Important: lamassu-migrate currently gripes about a QueryResultError. Ignore this, it works anyway.

```
node bin/lamassu-migrate
```

## Run new-lamassu-admin

```
cd new-lamassu-admin/
npm install
npm run start
```

## Run lamassu-admin-server

In a second terminal window:

```
node bin/lamassu-admin-server --dev
```

## Register admin user

In a third terminal window:

```
node bin/lamassu-register admin@example.com superuser
```

You'll use this generated URL in the brower in a moment.

## Complete configuration

Paste the URL from lamassu-register exactly as output, into a browser (chrome or firefox).

**Important**: the host must be localhost. Tell your browser to trust the certificate even though it's not signed by a recognized CA. If you get an "expired" error, try opening https://localhost:8070/graphql in another tab and trust the certificate.

Go to all the required, unconfigured red fields and choose some values. Choose mock services whenever available.

## Run lamassu-server

```
node bin/lamassu-server --mockSms
```

## Add a lamassu-machine

Click on ``+ Add Machine`` in the sidebar. Type in a name for your machine and click **Pair**. Open up development tools to show the JavaScript console and copy the totem. You will use this to run lamassu-machine. This pairing totem expires after an hour.

Now continue with lamassu-machine instructions from the ``INSTALL.md`` file in lamassu-machine.
