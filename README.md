[![Build Status](https://travis-ci.org/mongrelion/ansible-role-docker.svg?branch=master)](https://travis-ci.org/mongrelion/ansible-role-docker)

# docker

Install and configure Docker.

## Role Variables

### `docker_config`

A dict of options that are written into docker's `daemon.json` config file. See [the docs for dockerd](https://docs.docker.com/engine/reference/commandline/dockerd/) for a full list of available options.

Default values: (set them in your `docker_config` to overwrite)

    storage-driver: devicemapper
    log-level: info

### `docker_version`

Specify the version of Docker to install, e.g. `1.12.6`, `17.05`.

Default value: `17.03`

### `docker_setup_script_md5_sum`

Default value: md5 checksum of default `docker_version` setup script (see `defaults/main.yml` for exact default value)

**If you intend to install a version of Docker other than the default, you must provide an appropriate override value for this variable.**

Either:

1. Generate an md5 checksum for the desired version's install script
1. If you know what you are doing and are not worried about security, set this variable to "no" or "false" to disable checksum verification of the setup script.

### `docker_setup_script_url`

URL pointing to a Docker setup script that will install the specified `docker_version`.

Default value: `https://releases.rancher.com/install-docker/{{ docker_version }}.sh`

The default URL utilizes [Rancher Labs' version-specific, OS-agnostic setup scripts](https://github.com/rancher/install-docker), which in turn just install the appropriate version of `docker-ce` or `docker-engine` from the official Docker `apt` and `yum` repositories.

### `docker_upgrade`

Per default, this role will only download and run the installation script when
Docker is not installed (or more precise: when `dockerd` is not in `$PATH`). Set
`docker_upgrade` to `True` to override this behavior and force the install
script to be run.

So in order to upgrade Docker on managed systems, take the following steps:

0. Either download a newer version of this role (with a more recent default
   version) or update `docker_version` and `docker_setup_script_md5_sum` in your
   host/group vars.
1. Run your playbook with `-e docker_upgrade=True`

### `docker_proxy`, `docker_http_proxy`, `docker_https_proxy`, `docker_no_proxy`

`docker_proxy` specifies if proxy need to be applied. Default value of `docker_proxy` is no. If you need proxy set it to yes and updated other three variables as needed.

## Dependencies

None

## Example Playbook

Install Docker
```yaml
- hosts: servers
  roles:
    - mongrelion.docker
```

Install and configure docker
```yaml
- hosts: servers
  roles:
    - role: mongrelion.docker
      docker_config:
        live-restore: true
        userland-proxy: false
```

## Testing

The preferred way of locally testing the role is to use Docker and [molecule](https://github.com/metacloud/molecule) (v1.25). You will have to install Docker on your system. See Get started for a Docker package suitable to for your system.
All packages you need to can be specified in one line:
```sh
pip install ansible 'ansible-lint>=3.4.15' 'molecule==1.25.0' docker 'testinfra>=1.7.0,<=1.10.1'
```
This should be similar to one listed in `.travis.yml` file in `install` section. 
After installing test suit you can run test by running
```sh
molecule test
```
For more information about molecule go to their [docs](http://molecule.readthedocs.io/en/stable-1.25/).

## License

MIT

## Author Information

You can find me on Twitter: [@mongrelion](https://twitter.com/mongrelion)
