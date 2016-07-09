# ansible-role-cronie

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-cronie.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-cronie)

RHEL/CentOS - UNIX daemon crond (cronie)

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    cronie_deny: []
    cronie_hourly_mailto: root
    cronie_hourly_path: [ '/sbin', '/bin', '/usr/sbin', '/usr/bin' ]
    cronie_hourly_shell: /bin/bash
    cronie_jobs: ''
    cronie_sysconfig_args: ''

You can define cron jobs using the following variable:

    cronie_jobs:
      - dest: /etc/cron.d/ansible
        jobs:
          - interval: '* * * * *'
            owner: root
            job: 'echo cronie >/dev/null'

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.cronie
          cronie_deny:
            - tkimball
          cronie_jobs:
            - dest: /etc/cron.d/pwck
              jobs:
                - interval: '@hourly'
                  owner: root
                  job: '/usr/sbin/pwck -s'

## License

BSD

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
