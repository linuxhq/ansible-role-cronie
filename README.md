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

GPLv3

## Author Information

This role was created by [Taylor Kimball](http://www.linuxhq.org).
