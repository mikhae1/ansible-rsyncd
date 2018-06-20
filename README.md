# Rsync Server

Setup rsync daemon at Centos 7

## Role Variables

Defaults: `defaults/main.yml`
- `rsync_server_timeout`: Timeout for connections, default `300` seconds
- `rsync_server_max_connections`: Maximum number of connections, default `10`
- `rsync_server_shares`: A list of dictionaries of shares with fields:
  - `name`: The published name of the share, required
  - `path`: Filesystem path of the share, required
  - `comment`: Description of the share, default is the same as `name`
  - `readonly`: Whether the share is read-only, default `True`
  - `uid`: User name or ID when access the share, default `nobody`
  - `gid`: Group name or ID when access the share, default `nobody`
  - `excludes`: A list of exclusions, default `['lost+found']`
  - `auth_users`: Rsync auth users

## Playbook example

```yml
- hosts: localhost
  roles:
  - role: rsync-server
    rsync_server_shares:
    - name: dataset-1
      comment: Public datasets
      path: /data/1
```

## Server's rsync.secrets example

```
backup:12345
```

## Client example

```sh
$ RSYNC_PASSWORD=12345 rsync -hav rsync://backup@localhost/dataset-1 .
```

Using passord file:
```sh
$ echo '12345' > ./rsync.pass; chmod 0600 ./rsync.pass
$ rsync -hav --password-file=./rsync.pass rsync://backup@localhost/test .
```
