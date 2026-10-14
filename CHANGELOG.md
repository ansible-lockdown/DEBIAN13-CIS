# Changes to DEB13CIS

## CIS Benchmark v1.0.0
## October 2026 - QA fixes

- 2.2.1 nis removal gated on deb13cis_nis_client_required
- 5.4.1.5 deb13cis_inactivelock_include_root added to apply the inactive lock to root
- 6.1.1.1.x journald config directory created whatever the syslog choice (addresses public issue 21) - Thank you @moritzrp
- 6.1.1.1.6 dest no longer ends .j2, stray 60-journald.conf.j2 removed (addresses pblic issue 22) - Thank you @moritzrp
- 6.1.1.1.4 ForwardToSyslog=no applied only when deb13cis_syslog is journald
- 6.1.2.3 ForwardToSyslog change restarts journald
- 6.1.1.1.1 and 6.1.1.1.2 tagged patch
- 2.3.1.1 mask task uses ansible.builtin.systemd
- 2.2.1, 2.3.2.2 and 2.3.3.3 given the patch tag
- 1.3.1.4 sysctl.d find uses a glob pattern
- 1.3.1.3 reports changed only when the enforce count changes
- 1.5.9 find searches for kernel.randomize_va_space
- 1.2.1.6 auth.conf.d files set to 0640
- prelim apt repo file find uses a regex pattern
- prelim UID 0 discovery no longer fails when extra accounts exist
- 5.1.12 runs when 5.1.13 is skipped
- 5.1.13 KexAlgorithms line fixed, validated and restarts sshd
- 5.1.13 guarded when openssh-server is not installed
- 6.1.3.1 log file tasks match on item.path, stray failed_when removed
- 7.2.10 skipped when no interactive home directories exist
- pam-configs templates: managed-by-ansible header removed
- pam_unix.j2 Password line ends {% endif +%}
- pwquality.j2 Password-Initial module line added
- level tags corrected from the benchmark profiles (26 controls)
- uas removal moved out of tags: always, tagged rule_1.1.1.9
- section 5 rule toggles added to vars/is_container.yml
- 99_auditd.rules mode o-rwx
- Boot type detection moved before the pre-remediation audit
- Container discovery guarded when virtualization_type is undefined
- ansible_env and ansible_local replaced with ansible_facts
- compliance facts refresh reads post_audit_results
- no_log on the sudo user password hash lookup
- audit_bin_validate_certs wired to the binary download
- deb13cis_remote_log_server and deb13cis_config_aide passed to the audit
- 7.1.12 unowned search path no longer double-wrapped in the audit bridge
- unused registers removed
- stale section comments in defaults corrected
- Company name updated to MindPoint Group - A Quantum Sky Company
- audit binary switched from goss to syver v0.13.0
- prelim_goss_available renamed prelim_audit_bin_available
- README audit section describes syver
- changelog update

## Based on CIS v1.0.0 - Branch sept26_update

- sept26_update branch
  - 7.1.13 find used -perm \( ... or ... \); find read \( as the mode and never returned a
    file, so the SUID/SGID review passed vacuously. Now \( -perm -02000 -o -perm -04000 \)
  - 1.1.1.6 blacklisted overlayfs, which has not existed since the 3.x kernels. Now overlay,
    host-verified: modprobe -n -v overlay went from loading overlay.ko.xz to install /bin/true
  - 1.2.1.2 and 1.3.1.4 when: expressions were invalid Jinja (unquoted path, and not used as a
    binary operator) - both now quoted comparisons
  - prelim apt.conf.d find used patterns: .* as a glob, matching only dotfiles, so 1.2.1.2
    processed nothing. Added use_regex: true - host-verified 0 of 6 files matched before, 6 after
  - 3.2.1-3.2.6 modprobe tasks looped two lines against one fixed regexp, so the second item
    overwrote the first. Each item now carries its own regexp
  - 3.2.x install regexps were single-quoted, so \\s stayed literal and never matched
  - 3.1.2 wrote "install <mod> true" with a regexp naming "true"; now /bin/true and the module
  - 6.1.1.2.2 notified Restart journald, restarting the wrong service; now
    Restart systemd_journal_upload, which was previously unreachable
  - 7.2.7 warning referenced discovered_user_username_check, which is defined nowhere
  - removed a verbatim duplicate 3.2.3 dccp block
  - ansible_facts dot notation converted to bracket notation, 37 occurrences across 17 files
  - check_prereqs.yml when: reflowed to double quotes, the bracket form collides with a single-quoted scalar
  - deb13cis_shell_executable added to vars/main.yml, the role had no shell executable var
  - set -o pipefail and args executable applied to all 64 shell tasks
  - 4 folded '>' shell scalars in prelim.yml converted to literal '|', pipefail cannot work in a folded block
  - 34 hardcoded executable /bin/bash replaced with the role var
  - risky-shell-pipe removed from the .ansible-lint skip_list, the role now passes with it enabled
  - fixed the collection of audit privileged commands
  - 6.2.3.10 registered discovered_privileged_commands but the template reads discovered_privilege_processes, so no rules were written
  - Audit: 6.2.3.10 placeholder replaced with conf and running checks

## Based on CIS v1.0.0 - Branch align_1.0.0

- 7.2.9 set default ACLs on home directories but never removed excessive permissions from the
  directory itself, so /home/<user> stayed at 0755 while the audit expects 0750 or tighter
- Fix 12 audit files gated on the wrong deb13cis_rule_* toggle
- Align 2.1.22 title with the benchmark and correct the leading ID in the audit title
- 1.1.2.1.1 /tmp fix thans to @tomtrix PR #8
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
- audit variable moved to defaults/main/audit.yml for easier override
- fixed optional uas #12 thanks @masterjanic
- README updates and updated contributing and contributors

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
