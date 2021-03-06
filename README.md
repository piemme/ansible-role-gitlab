# Ansible Role: GitLab

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-gitlab.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-gitlab)

Installs GitLab, a Ruby-based front-end to Git, on any RedHat/CentOS or Debian/Ubuntu linux system.

GitLab's default administrator account details are below; be sure to login immediately after installation and change these credentials!

    root
    5iveL!fe

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    gitlab_external_url: "https://gitlab/"

The URL at which the GitLab instance will be accessible. This is set as the `external_url` configuration setting in `gitlab.rb`, and if you want to run GitLab on a different port (besides 80/443), you can specify the port here (e.g. `https://gitlab:8443/` for port 8443).

    gitlab_git_data_dir: "/var/opt/gitlab/git-data"

The `gitlab_git_data_dir` is the location where all the Git repositories will be stored. You can use a shared drive or any path on the system.

    gitlab_backup_path: "/var/opt/gitlab/backups"

The `gitlab_backup_path` is the location where Gitlab backups will be stored.

    gitlab_edition: "gitlab-ce"

The edition of GitLab to install. Usually either `gitlab-ce` (Community Edition) or `gitlab-ee` (Enterprise Edition).

    gitlab_version: ''

If you'd like to install a specific version, set the version here (e.g. `10.0.6-ce.0` for Debian/Ubuntu, or `10.0.6-ce.0.el7` for RedHat/CentOS).

    # SSL Configuration.
    gitlab_redirect_http_to_https: "true"
    gitlab_ssl_certificate: "/etc/gitlab/ssl/gitlab.crt"
    gitlab_ssl_certificate_key: "/etc/gitlab/ssl/gitlab.key"

GitLab SSL configuration; tells GitLab to redirect normal http requests to https, and the path to the certificate and key (the default values will work for automatic self-signed certificate creation, if set to `true` in the variable below).

    # SSL Self-signed Certificate Configuration.
    gitlab_create_self_signed_cert: "true"
    gitlab_self_signed_cert_subj: "/C=US/ST=Missouri/L=Saint Louis/O=IT/CN=gitlab"

Whether to create a self-signed certificate for serving GitLab over a secure connection. Set `gitlab_self_signed_cert_subj` according to your locality and organization.

    # LDAP Configuration.
    gitlab_ldap_enabled: "false"
    gitlab_ldap_host: "example.com"
    gitlab_ldap_port: "389"
    gitlab_ldap_uid: "sAMAccountName"
    gitlab_ldap_method: "plain"
    gitlab_ldap_bind_dn: "CN=Username,CN=Users,DC=example,DC=com"
    gitlab_ldap_password: "password"
    gitlab_ldap_base: "DC=example,DC=com"

GitLab LDAP configuration; if `gitlab_ldap_enabled` is `true`, the rest of the configuration will tell GitLab how to connect to an LDAP server for centralized authentication.

    gitlab_time_zone: "UTC"

Gitlab timezone.

    gitlab_backup_keep_time: "604800"

How long to keep local backups (useful if you don't want backups to fill up your drive!).

    gitlab_download_validate_certs: yes

Controls whether to validate certificates when downloading the GitLab installation repository install script.

    # Email configuration.
    gitlab_email_enabled: "false"
    gitlab_email_from: "gitlab@example.com"
    gitlab_email_display_name: "Gitlab"
    gitlab_email_reply_to: "gitlab@example.com"

Gitlab system mail configuration. Disabled by default; set `gitlab_email_enabled` to `true` to enable, and make sure you enter valid from/reply-to values.

    # SMTP Configuration
    gitlab_smtp_enable: "false"
    gitlab_smtp_address: "smtp.server"
    gitlab_smtp_port: "465"
    gitlab_smtp_user_name: "smtp user"
    gitlab_smtp_password: "smtp password"
    gitlab_smtp_domain: "example.com"
    gitlab_smtp_authentication: "login"
    gitlab_smtp_enable_starttls_auto: "true"
    gitlab_smtp_tls: "false"
    gitlab_smtp_openssl_verify_mode: "none"
    gitlab_smtp_ca_path: "/etc/ssl/certs"
    gitlab_smtp_ca_file: "/etc/ssl/certs/ca-certificates.crt"

Gitlab SMTP configuration; of `gitlab_smtp_enable` is `true`, the rest of the configuration will tell GitLab how to send mails using an smtp server.

    gitlab_nginx_listen_port: 8080

If you are running GitLab behind a reverse proxy, you may want to override the listen port to something else.

    gitlab_nginx_listen_https: "false"

If you are running GitLab behind a reverse proxy, you may wish to terminate SSL at another proxy server or load balancer

    gitlab_nginx_ssl_verify_client: ""
    gitlab_nginx_ssl_client_certificate: ""

If you want to enable [2-way SSL Client Authentication](https://docs.gitlab.com/omnibus/settings/nginx.html#enable-2-way-ssl-client-authentication), set `gitlab_nginx_ssl_verify_client` and add a path to the client certificate in `gitlab_nginx_ssl_client_certificate`.

    gitlab_nginx_worker_processes: 4
    gitlab_nginx_worker_connections: 10240

Gitlab nginx worker settings.

    gitlab_default_theme: 2

If you want to set default theme for all users.

    gitlab_logrotate_enable: "true"
    gitlab_logrotate_frequency: "daily"
    gitlab_logrotate_size: ""
    gitlab_logrotate_rotate: 30
    gitlab_logrotate_compress: "compress"
    gitlab_logrotate_method: "copytruncate"
    gitlab_logrotate_postrotate: ""
    gitlab_logrotate_dateformat: ""

You can disable the built-in logrotate service if you want (enable by default).
If you want to set some [Gitlab logrotate settings](https://docs.gitlab.com/omnibus/settings/logs.html#logrotate), you need to specify `gitlab_logrotate_enable: true`.

    gitlab_svlogd_size: 200 * 1024 * 1024
    gitlab_svlogd_num: 30
    gitlab_svlogd_timeout: 24 * 60 * 60
    gitlab_svlogd_filter: "gzip"
    gitlab_svlogd_udp: ""
    gitlab_svlogd_prefix: ""

You can modify the [runit Logs](https://docs.gitlab.com/omnibus/settings/logs.html#runit-logs) with these svlogd settings.

    gitlab_prometheus_monitoring_enable: 'true'

To completely disable [Gitlab Prometheus](https://docs.gitlab.com/ce/administration/monitoring/prometheus), and all of it's exporters, set to false.

    gitlab_monitor_enable: 'true'
    gitlab_monitor_log_directory: '/var/log/gitlab/gitlab-monitor'
    gitlab_monitor_home: '/var/opt/gitlab/gitlab-monitor'
    gitlab_monitor_listen_address: 'localhost'
    gitlab_monitor_listen_port: '9168'

[GitLab monitor exporter](https://docs.gitlab.com/ce/administration/monitoring/prometheus/gitlab_monitor_exporter.html) configuration; if `gitlab_monitor_enable` is `true` the rest of the configuration could be set.

    gitlab_prometheus_enable: 'true'
    gitlab_prometheus_monitor_kubernetes: 'true'
    gitlab_prometheus_username: 'gitlab-prometheus'
    gitlab_prometheus_uid: ''
    gitlab_prometheus_gid: ''
    gitlab_prometheus_shell: '/bin/sh'
    gitlab_prometheus_home: '/var/opt/gitlab/prometheus'
    gitlab_prometheus_log_directory:  '/var/log/gitlab/prometheus'
    gitlab_prometheus_scrape_interval: 15
    gitlab_prometheus_scrape_timeout: 15
    gitlab_prometheus_chunk_encoding_version : 2

If you want to set some [Prometheus settings](https://docs.gitlab.com/ce/administration/monitoring/prometheus/) configuration; if `gitlab_prometheus_enable` is set the rest of the configuration could be set.

    gitlab_prometheus_listen_address: 'localhost:9090'

If you want to set the [port Prometheus listen](https://docs.gitlab.com/ce/administration/monitoring/prometheus/#changing-the-port-prometheus-listens-on). Could be useful to change with an IP if you use a platform for analytics and monitoring.

    gitlab_packages_dependencies:
      - openssh-server
      - postfix
      - curl
      - openssl
      - tzdata

If you want to add or remove some packages dependencies.

## Dependencies

None.

## Example Playbook

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - { role: geerlingguy.gitlab }

*Inside `vars/main.yml`*:

    gitlab_external_url: "https://gitlab.example.com/"

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
