# Command Line Tool (lightflus-cli)

## GETTING STARTED

### Syntax

```shell
lightflus [cmd] [TYPE] [NAME] [flags]
```

where `cmd`, `TYPE`, `NAME` and `flags` are:

* `cmd`: Specifies the operation that you want to perform, for example `create`, `get`, `describe`,
  `delete`.
* `TYPE`: Specifies the resource type.Resource types are case-insensitive such as `task`, `namespace`.
* `NAME`: Specifies the name of the resource. Names are case-sensitive. If the name is omitted, details for all
  resources are displayed, for example `lightflus get tasks`.
* `flags`: Specifies optional flags. For example, you can use the `-s` or `--server` flags to specify the address and
  port of the Lightflus API server.

### Authentication

By default, Lightflus will use authentication information in local configuration cached file located at
path `${HOME}/.lightflus/config`. It first checks if environment variables `LIGHTFLUS_SERVICE_HOST`
and `LIGHTFLUS_SERVICE_PORT` exist then Lightflus will log in remote service automatically when you complete the
authentication.

**How you complete the authentication**

You can run command like:

```bash
lightflus config --path=${HOME}/.lightflus/config set-credential --username=your_username --password=your_pwd
```

**How lightflus handle your authentication**

If:

* environment variables `LIGHTFLUS_SERVICE_HOST` is set;
* environment variables `LIGHTFLUS_SERVICE_PORT` is set;
* token has been cached in config file `${HOME}/.lightflus/config`

## OPERATIONS
