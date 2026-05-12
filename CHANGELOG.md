# Changes to DEB13CIS

## Based on CIS v1.0.0 - Branch 2026_MAY_QA

- Update min_ansible_version to 2.16.1
- Remove incorrect 'stig' galaxy tag
- Fix LICENSE company name casing (MindPoint)
- Standardize changelog filename to CHANGELOG.md
- Remove export_badges_public.yml from private repo
- Add community.docker.docker to container detection
- Rewrite is_container.yml with correct Deb13 CIS rule IDs
- Add missing handlers: Update_Initramfs, Restart syslog service
- Add container guards to auditd handlers
- Add sshd -t validation to Restart sshd handler
- Fix circular register/failed_when in Auditd rules reload handler
- Add args: executable: /bin/bash to piped shell commands (Debian uses dash)
- Fix register order (register after changed_when/failed_when)
- Add no_log to shadow file tasks
- Add file_managed_by_ansible header to audit template
- Fix SELinux references to AppArmor
- Fix sudoers use_pty backrefs silent skip on fresh systems
- Remove duplicate Restart rsyslog handler (orphaned)
- Convert sshd handler from block to listen pattern (Ansible 2.19 compatibility)
- Add ufw to molecule prepare packages
- Add kernel module, AppArmor GRUB, wireless/network rules to is_container.yml
- Update meta author to Ansible-Lockdown Team
- Create molecule scenarios (default, localhost, wsl) with audit enabled
- Fix check_prereqs.yml: replace libselinux with python3-apt

### March 2026 - Common alignment and validation

- Synced common files
- Titles and company references aligned
- fixed meta
- variable naming aligned across remediate and audit
  - deb13cis_gui variable renamed to deb13cis_desktop_required
  - deb13_time_pool_name to deb13cis_time_pool
- standard common file updates
- unique variable naming
- some tasks and handlers updated

## 1.0.0 Initial
