# Changes to DEB13CIS

## Based on CIS v1.0.0 - Branch align_1.0.0

- Fix 12 audit files gated on the wrong deb13cis_rule_* toggle
- Align 2.1.22 title with the benchmark and correct the leading ID in the audit title
- Remove /etc/sysctl.conf from section 1.5 sysctl handling, no longer honored on Debian 13, and add /etc/ufw/sysctl.conf (thanks @seven-beep, PR #6)
- Remove three dead duplicate-named 1.5.4 / 1.5.8 / 1.5.9 replace tasks (thanks @seven-beep, PR #6)
- Fix self-referential changed_when / failed_when on the 1.5.1, 1.5.2 and 1.5.5 replace tasks, now driven by a stat check
- Write 5.1.16 MACs to deb13cis_sshd_config_file instead of a hardcoded path, and disambiguate duplicate 5.1.1 task names (thanks @seven-beep, PR #5)
- Guard the /etc/network/interfaces audit rule behind a stat check so rule load no longer fails on cloud images (thanks @seven-beep, PR #7)
- Make /tmp handlers mutually exclusive on deb13cis_tmp_svc, and reload tmp.mount rather than restart it (thanks @seven-beep, PR #4)
- Fix 5.4.1.1 discovery awk comparing PASS_MAX_DAYS against deb13cis_pass_min_days, users with a max age between 1 and 364 were never detected
- Fix unbalanced quote in the 5.4.1.1 Warn count task name
- Remove 3 ghost toggles with no task body - deb13cis_force_user_maxdays, _mindays, _warnage
- Correct 5 inverted automated/manual tags in section 6.1.2
- Relabel deb13cis_passwd_hash_algo as 5.4.1.4 and deb13cis_bash_umask as 5.4.3.3, and move both into their correct section blocks
- Rename audit bridge template ansible_vars_goss.yml.j2 to lockdown_audit.yml.j2
- Add missing file_managed_by_ansible header to 6 templates
- Correct variable name in the UAS warning message, debian13cis_uas_remove -> deb13cis_uas_remove
- Inline a single-item when: list in prelim.yml
- Correct the social badge label from "Twitter URL" to "X URL"
- Add test_inv to .gitignore
- Bump actions/checkout to v7.0.0 in both pipeline workflows
- Update 7.2.9 acl check to use interactive users only
- Populate prelim_interactive_users in prelim.yml, it was an empty list so the 7.2.9 ACL tasks silently never applied
- Add gated PRELIM install of acl, Debian minimal does not ship it
- Notify the ipv4/ipv6 route flush handlers from post.yml, 3.3.1.16 and 3.3.1.17 were written to /etc/sysctl.d but never applied at runtime
- Move Reload sysctl above both flush handlers, handlers run in definition order so the flush ran before the values it applies
- Align the two log_martians entries in /etc/ufw/sysctl.conf from post.yml, ufw reverted 3.3.1.16 and 3.3.1.17 at every boot
- Parameterise 5.4.1.5 account selection against deb13cis_inactivelock_lock_days, skip locked accounts, and correct the title to "is configured"
- Apply 6.2.4.1 mode to the audit log files rather than recursing the log directory, which left /var/log/audit at 0640
- Set 6.2.4.4 mode to u+rwx,g+rx,g-w,o-rwx so it converges on 0750 and repairs hosts already left at 0640
- Glob /usr/share/pam-configs/* in 5.3.3.4.2 and 5.3.3.4.3, the hardcoded pam_unix path does not exist on stock Debian 13

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
