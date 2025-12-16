# Changes to deb12CIS

## 1.2.1 Based on CIS v1.1.0
Aug25 Updates

- ansible audit template update for better var usage and aligned with audit
- 1.3.1.4 updated to make ansible-core 2.19 compliant
- 5.3.1.x updated logic improved and ansible-core 2.19 compliant

- thanks to @polski-g
  - issue 85 from public
- thanks to dderemiah
  - issue 98
    - dconf handler name update
    - typo in mode 1.7.2
  - issue 99
    - rsync service name
    - handler systemd_daemon_reload rename
- thanks to @matt-j-griffin
  - auditd template updated for ansible-core 2.19

## 1.2.0 Based on CIS v1.1.0

Oct 25

- added fixes thanks to @tomtrix and @dvic #96
- added fix for #101 thanks to @aveset
- audit files updated and max-concurrent option added
- workflow updates
- README update
- updated 1.1.1.7 thanks to @aderumier
- audit rules logic updated

July 2025 QA Updates
  - Fix for #89 and #90, thank you @Arvyr

June 2025 QA Updates
  - Update to audit_only to allow fetching results
  - resolved false warning for fetch audit
  - Improved documentation and variable compilation for crypto policies

## 1.1.0 Based on CIS v1.1.0
May 2025
Thank you @DianaMariaDDM
 - Decoupling sshd vars in defaults/main. Adresses #40
Thank you @polski-g for the following:
  - discovered_group_check fix for 7.2.8 based on PR #72
  - failed_when logic improvement based on PR #73
  - /etc/systemd/journald.conf.d improvement on logic for directory existence based on PR #74
  - task/prelim check_mode logic improvement based on PR #75
  - shell logic fix from getent passwd to getent group based on PR #76
- QA Typo Fixes and .github update

Rules updated, removed, moved(renumbered), updated

new functions added
- ansible.facts created
- ability to fetch/copy audit logs to centralised location
Alignment with 1.0.1 in public

## 0.9.0- Based on CIS v1.0.1

- Initial release of playbook
