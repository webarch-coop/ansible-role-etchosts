# Webarchitects /etc/hosts Ansible role

[![pipeline status](https://git.coop/webarch/etchosts/badges/main/pipeline.svg)](https://git.coop/webarch/etchosts/-/commits/main)

An Ansible role to configure the contents of `/etc/hosts`.

By default this role edits, rather than templating the `/etc/hosts` file, this has the advantage that it won't remove comments or existing lines (when not asked to), however in this mode it can't re-order lines, set `etchosts_template` to `true` for the `/etc/hosts` file to be templated.

## Usage

The existing content of `/etc/hosts` can be read as YAML using [JC](https://kellyjonbrazil.github.io/jc/):

```bash
cat /etc/hosts | jc --hosts -y
```
```yaml
  - ip: 127.0.0.1
    hostname:
      - localhost
  - ip: ::1
    hostname:
      - ip6-localhost
      - ip6-loopback
```

The result returned when `jc --hosts -y` is used to parse the `/etc/hosts` file can be directly used for the `etchosts_file` array, with the optional addition of a `state` variable that can be added to each item and set to `present` (the default) or `absent`.

## Role variables

See the [defaults/main.yml](defaults/main.yml) file for the default variables, the [vars/main.yml](vars/main.yml) file for the preset variables and the [meta/argument_specs.yml](meta/argument_specs.yml) file for the variable specification.

### etchosts

A boolean that defaults to `false`, set `etchosts` to `true` for the tasks in this role to be run.

### etchosts_file

A list of entries for the `/etc/hosts` file in the same format as returned by the [JC hosts parser](https://kellyjonbrazil.github.io/jc/docs/parsers/hosts), plus an optional `state` variable, for example:

```yaml
etchosts_file:
  - ip: 192.168.0.1
    state: absent
  - ip: 192.168.0.2
    hostname:
      - foo.example.lan
      - foo
```
The `ip` address is required, the `hostname` is only required when the `state` is not defined or set to `present`.

### etchosts_hostname

The domain name for `/etc/hostname`, if blank the hostname will not be set.

### etchosts_hostname_strategy

The strategy for [ansible.builtin.hostname](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/hostname_module.html) to use.

### etchosts_mailname

The domain name for `/etc/mailname`, if blank the mailname will not be set.

### etchosts_template

A boolean that defaults to `false` which results in the `/etc/hosts` file being edited using [ansible.buiiltin.lineinfile](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html), set `etchosts_template` to `true` for [ansible.builtin.template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) to be used.

### etchosts_validate

A boolean that defaults to `true`, which results in the [meta/argument_specs.yml](meta/argument_specs.yml) file being used to check all variables that start with `etchosts_`.

## Repository

The primary URL of this repo is [`https://git.coop/webarch/etchosts`](https://git.coop/webarch/etchosts) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-etchosts) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/etchosts).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/etchosts/-/releases).

## Copyright

Copyright 2023 Chris Croome, &lt;[chris@webarchitects.co.uk](mailto:chris@webarchitects.co.uk)&gt;.

This role is released under [the same terms as Ansible itself](https://github.com/ansible/ansible/blob/devel/COPYING), the [GNU GPLv3](LICENSE).
