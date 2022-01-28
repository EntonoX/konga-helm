# Konga Helm

This helm chart contains the deployment of Konga. The Parameters section lists the parameters that can be configured
during installation.

# Requirements

Requirements are;

- GKE Cluster
- MySQL instance with database and user created
- Database secret

Create secrets

```bash
$ kubectl create secret generic konga-db-credentials --from-literal=username=<USERNAME> --from-literal=password='<PASSWORD>'
```

and provide the correct credentials. If secret name vary from above, please set the right names as option or
in a provided values file.

# Installing the Chart

To install the chart with the release name `konga`:

```bash
$ helm install konga konga/konga
``` 

This will install the application with default values. Use the  `--set` option to set variables or provide a custom
values file with `-f values.yaml`

# Uninstalling the Chart

To uninstall/delete the `konga` deployment:

```bash
$ helm delete konga
```

# Upgrading the Chart

To upgrade the `konga` installation:

```bash
 $ helm upgrade konga -f VALUES_FILE .
```

# Packaging the Chart

When changes are made to this Helm chart, package it with:

```bash
$ helm package .
$ helm repo index .
```

# Helm repository

Because this is a private repository, create a `App password` in BitBucket with read permissions on the repository. Then
this helm chart can be added as a private Helm repository with:

```bash
$ helm repo add authorization https://api.bitbucket.org/2.0/repositories//samr-leusden/konga-helm/src/master/ --username <USERNAME> --password <APP-PASSWORD>
```

replace `<USERNAME>` and `<PASSWORD>` with correct bitbucket credentials

# Parameters

This section describes all the available parameters in the default `values.yaml`

| VAR                | DESCRIPTION                                                                                                                | VALUES                                 | DEFAULT                                      |
|--------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------|----------------------------------------------|
| HOST               | The IP address that will be bind by Konga's server                                                                               | -                                      | '0.0.0.0'                                         |
| PORT               | The port that will be used by Konga's server                                                                               | -                                      | 1337                                         |
| NODE_ENV           | The environment                                                                                                            | `production`,`development`             | `production`                                |
| SSL_KEY_PATH       | If you want to use SSL, this will be the absolute path to the .key file. Both `SSL_KEY_PATH` & `SSL_CRT_PATH` must be set. | -                                      | null                                         |
| SSL_CRT_PATH       | If you want to use SSL, this will be the absolute path to the .crt file. Both `SSL_KEY_PATH` & `SSL_CRT_PATH` must be set. | -                                      | null                                         |
| KONGA_HOOK_TIMEOUT | The time in ms that Konga will wait for startup tasks to finish before exiting the process.                                | -                                      | 60000                                        |
| DB_ADAPTER         | The database that Konga will use. If not set, the localDisk db will be used.              | `mongo`,`mysql`,`postgres`     | -                                            | 'mysql'
| DB_URI             | The full db connection string. Depends on `DB_ADAPTER`. If this is set, no other DB related var is needed.                 | -                                      | -                                            |
| DB_HOST            | If `DB_URI` is not specified, this is the database host. Depends on `DB_ADAPTER`.                                          | -                                      | 1.2.3.4                               |
| DB_PORT            | If `DB_URI` is not specified, this is the database port. Depends on `DB_ADAPTER`.                                         | -                                      | 3306                                  |
| DB_USER            | If `DB_URI` is not specified, this is the database user. Depends on `DB_ADAPTER`.                                          | -                                      | -                                            |
| DB_PASSWORD        | If `DB_URI` is not specified, this is the database user's password. Depends on `DB_ADAPTER`.                               | -                                      | -                                            |
| DB_DATABASE        | If `DB_URI` is not specified, this is the name of Konga's db. Depends on `DB_ADAPTER`.                                    | -                                      | `konga`                             |
| DB_PG_SCHEMA       | If using postgres as a database, this is the schema that will be used.                                                     | -                                      | `public`                                     |
| KONGA_LOG_LEVEL    | The logging level                                                                                                          | `silly`,`debug`,`info`,`warn`,`error`  | `debug` on dev environment & `warn` on prod. |
| TOKEN_SECRET       | The secret that will be used to sign JWT tokens issued by Konga | - | - |
| NO_AUTH            | Run Konga without Authentication                                                                                           | true/false                             | -                                         |
| BASE_URL           | Define a base URL or relative path that Konga will be loaded from. Ex: www.example.com/konga                               | <string>                                     | -                                         |
| KONGA_SEED_USER_DATA_SOURCE_FILE           | Seed default users on first run. [Docs](./docs/SEED_DEFAULT_DATA.md).                               | <string>                                     | -                                         |
| KONGA_SEED_KONG_NODE_DATA_SOURCE_FILE      | Seed default Kong Admin API connections on first run [Docs](./docs/SEED_DEFAULT_DATA.md)                               | <string>                                     | -                                         |

