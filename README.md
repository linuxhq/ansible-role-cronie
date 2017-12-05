# ansible-role-cronie

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-cronie.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-cronie)

RHEL/CentOS - UNIX daemon crond (cronie)

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    cronie_allow: []
    cronie_deny: []
    cronie_hourly_home: /
    cronie_hourly_mailto: root
    cronie_hourly_path: [ '/sbin', '/bin', '/usr/sbin', '/usr/bin' ]
      - /sbin
      - /bin
      - /usr/sbin
      - /usr/bin
    cronie_hourly_shell: /bin/bash
    cronie_jobs: {}
    cronie_sysconfig_args: ''

Below is example syntax of how to configure cronie jobs:

    cronie_jobs:
      - cron_file: linuxhq
        jobs:
          - name: Echo cronie to /dev/null every minute
            day: '*'
            hour: "*"
            job: 'echo cronie >/dev/null'
            minute: '*'
            month: '*'
            state: present
            user: root
            weekyday '*'

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.cronie
          cronie_allow:
            - tkimball
          cronie_jobs:
            - cron_file: linuxhq
              jobs:
                - name: Sort entries in /etc/group and /etc/gshadow by GID
                  job: '/usr/bin/sleep $[RANDOM\%3600] && /usr/sbin/grpck -s'
                  special_time: hourly
                - name: Sort entries in /etc/passwd and /etc/shadow by UID
                  job: '/usr/bin/sleep $[RANDOM\%3600] && /usr/sbin/pwck -s'
                  special_time: hourly

## License

GPLv3

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
