Role Name
=========

This role allows for deployment and configuration of GitLab CE

Requirements
------------

None

Role Variables
--------------

Variables from default directory :
* domain: domain belonging to customer
* git_url: URL on which GitLab will be listening
* Mail configuration :
  * mailserver: SMTP server to use for sending e-mails (defaults to smtp.{{ domain }})
  * smtpport: SMTP server port (defaults to 465)
  * smtpuser: SMTP username (defaults to smtpuser)
  * smtppass: SMTP user password (defaults to veryUnsecurePassToBeModified)
  * git_mail_from: from address used in e-mail sent from GitLab (defaults to git@{{ domain }})
  * default_maintenance_email: maintenance e-mail used to request Let's Encrypt certificate (defaults to maintenance@{{ domain }})
* OPTIONAL - SSO integration :
  * enable_omniauth: whether or not configure SSO integration (defaults to false)
  * sso_url: URL for SSO server
  * sso_oidc_gitlab_id: OpenID connect identifier defined for gitlab
  * sso_oidc_gitlab_secret: OpenID connect secret defined for gitlab
* OPTIONAL - Backups (for backups to be deployed, host needs to be in maintenance_contract group):
  * swift parameters for 2 object storage instances where backups should be pushed daily
  * git_backup_pass : Passphrase for encryption of backups

Variables from vars directory :
* gitlab_gpg_key_url: GitLab GPG key to be added to apt_key
* gitlab_packages_url: GitLab packages URL to retrieve packages from
* packages_to_install: GitLab Community Edition to be installed
* tmp_backup_dir: Temporary backup directory used during backup process
* backup_crons: list of scripts to be executed for backups together with associated cron tasks

Dependencies
------------

This role has no dependencies per-se, however, it is foreseen to use other roles from Le Filament on the same server :
* init_server for initial server configuration (and in particular SSHD configuration) - https://sources.le-filament.com/lefilament/ansible-roles/init_server
* security to securize GitLab server (iptables, auditd, fail2ban) - https://sources.le-filament.com/lefilament/ansible-roles/server_security

Example Playbook
----------------

    - hosts: gitlab
      become: true
      roles:
      - { role: gitlab, tags: gitlab }
      vars:
      - { domain: "example.org" }
      - { git_url: "git.{{ domain }}" }
      - { mailserver: "smtp.{{ domain }}" }
      - { smtpport: 465 }
      - { smtpuser: "smtpuser" }
      - { smtppass: "veryUnsecurePassToBeModified" }
      - { git_mail_from: "git@{{ domain }}" }
      - { default_maintenance_email: "maintenance@{{ domain }}" }

License
-------

AGPL-3

Author Information
------------------

Le Filament (https://le-filament.com)
