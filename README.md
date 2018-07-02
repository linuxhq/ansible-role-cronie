# ansible-role-cronie

[![Build Status](https://travis-ci.org/linuxhq/ansible-role-cronie.svg?branch=master)](https://travis-ci.org/linuxhq/ansible-role-cronie)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cronie-blue.svg?style=flat)](https://galaxy.ansible.com/linuxhq/cronie)
[![License](https://img.shields.io/badge/license-GPLv3-brightgreen.svg?style=flat)](COPYING)

RHEL/CentOS - UNIX daemon crond (cronie)

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    cronie_allow: []
    cronie_deny: []
    cronie_hourly_home: /
    cronie_hourly_mailto: root
    cronie_hourly_path:
      - /sbin
      - /bin
      - /usr/sbin
      - /usr/bin
    cronie_hourly_shell: /bin/bash
    cronie_sysconfig_args: ''
    cronie_system: []
    cronie_user: []

Example syntax for setting system and user environments and cronjobs:

    cronie_system:
      - cron_file: linuxhq
        environment:
          - name: path
            value: /bin:/usr/bin
        commands:
          - name: Lets echo 0 to /dev/null on boot
            job: 'echo 0 > /dev/null'
            special_time: reboot

    cronie_user:
      - user: tkimball
        environment:
          - name: path
            value: /bin:/usr/bin
        commands:
          - name: Lets echo 1 to /dev/null every minute
            job: 'echo 0 > /dev/null'

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
        - role: linuxhq.cronie
          cronie_allow:
            - tkimball
          cronie_system:
            - cron_file: linuxhq
              mode: '0600'
              environment:
                - name: path
                  value: /bin:/usr/bin
              commands:
                - name: Disable module loading after boot time
                  job: 'echo 1 > /proc/sys/kernel/modules_disabled'
                  special_time: reboot
                - name: Hide process information for other users
                  job: 'mount -o remount,hidepid=2,gid=0 /proc'
                  special_time: reboot
                - name: Purge RPM artifacts with rpmnew extension
                  job: 'sleep $[RANDOM\%3600] && find / -type f -name *.rpmnew -exec /bin/rm {} \; 2>/dev/null'
                  special_time: hourly
                - name: Purge RPM artifacts with rpmsave extension
                  job: 'sleep $[RANDOM\%3600] && find / -type f -name *.rpmsave -exec /bin/rm {} \; 2>/dev/null'
                  special_time: hourly
                - name: Sort entries in /etc/group and /etc/gshadow by GID
                  job: 'sleep $[RANDOM\%3600] && grpck -s'
                  special_time: hourly
                - name: Sort entries in /etc/passwd and /etc/shadow by UID
                  job: 'sleep $[RANDOM\%3600] && pwck -s'
                  special_time: hourly

## License

Copyright (C) 2018 Taylor Kimball <tkimball@linuxhq.org>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
