# Ansible Role: JFR
This role is designed to remotely execute Java Flight Recordings on hosts. It will also check for long running recordings triggered by web applications and stop those recordings.

## Requirements
Oracle/java environment variables set for play

Must run as user with permissions to and java directories

## Dependencies
None.

## Role Handlers
| Name          | Type    | Description          | File     |
| ------------- | ------- | -------------------- | -------- |
| notify jfrs killed | Slack | Notify slack channel when kill task is run | main.yml |

## Role Variables
| File | Name | Type | Description |
| -----| ---- | ---- | ----------- |
| vars/main.yml | | | |
| | JFR_PROFILE_SRC | path | name of JFR profile file in role |
| | JFR_PROFILE_DEST | path | path on remote host where profile file will be written |
| | JCMD | path | path to java JCMD binary |
| | JFR_File_Dir | path | path to location when jfr filename will be written |
| | JFR_Filename | string | name of jfr file |
| | JFR_Compress | boolean | true/false if file will be compressed |
| | JFR_Settings | path | path to the profile that will be used for the recording |
| | JFR_Duration | string | timespan, in seconds for which the recording will run. syntax requires time unit (e.g. 300s) |
| | JFR_NAME_SEARCH | string | grep search string to find the name of a specific recording revealed in the JFR.check command |
| | SLACK_CHANNEL | string | channel to which handler slack notification will be sent |
| defaults/main.yml | | |
| | channel_name | string | slack channel token |

## Current Entries/Tasks
|  Name  |  Hosts | File |
| -------|--------|------|
| | any |debug_jfr.yml| 
| | any |JFR_start.yml|

## Playbooks
- JFR_start.yml
- debug_jfr.yml

## Example Playbook
```
- name: run a jfr 
  hosts: all 
  become: yes
  become_user: vbms.wl
  environment:
    JAVA_HOME: /opt/java/jdk
  tasks:
    - import_role:
        name: ../roles/jfr
        tasks_from: JFR_start.yml
```

## Ansible Tower Info

### Job Templates
| Job | Playbook | Schedule |
| --- | -------- | -------- |
| JFR_Morning_Check | debug_jfr.yml | every morning at 05:00 EST |
| JFR_Evening_Check | debug_jfr.yml | every evening at 16:00 EST |
| JFR_start | JFR_start.yml | none |
| JFR_start_scheduled | JFR_start.yml | on demand |

#### Author Information
This role was created in 2020 by [Jared Zieche](https://github.com/JaredZieche)
