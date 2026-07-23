# WordPress application stack for Kubernetes on Wodby

Deploy WordPress applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for WordPress.

- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Start from a template

Use one of the compatible source templates exposed by this stack's services to
start with Wodby CI build configuration:

- [Vanilla WordPress](https://github.com/wodby/wordpress-vanilla)

## Service definitions

- [PHP (WordPress) service](https://github.com/wodby/service-wordpress-php)
- [Vinyl (WordPress) service](https://github.com/wodby/service-wordpress-vinyl)
- [Ganesha NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [Nginx (WordPress) service](https://github.com/wodby/service-wordpress-nginx)
- [MariaDB service](https://github.com/wodby/service-mariadb)
- [Valkey service](https://github.com/wodby/service-valkey)
- [Mailpit service](https://github.com/wodby/service-mailpit)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)
- [Gotenberg service](https://github.com/wodby/service-gotenberg)
- [Cloud MariaDB service](https://github.com/wodby/service-cloud-mariadb)
- [Cloud MySQL service](https://github.com/wodby/service-cloud-mysql)

## What's included

| Component | Purpose | Default configuration |
| --- | --- | --- |
| WordPress PHP | WordPress runtime | Required; PHP 8.4 by default, with PHP 8.3 and 8.5 available; 512 MB PHP memory limit |
| Nginx | Web server in front of PHP | Required |
| Vinyl | HTTP caching proxy in front of Nginx | Enabled and optional |
| NFS provisioner | Shared storage for `wp-content` | Enabled and optional; 25 GB data volume |
| MariaDB | WordPress database | Enabled and optional; 10 GB data volume |
| Valkey | Object cache | Enabled and optional |
| Mailpit | Email testing | Enabled and optional |
| Gotenberg | Document and PDF conversion | Enabled and optional |
| OpenSMTPD | Outbound email | Disabled and optional |
| Cloud MariaDB / MySQL | External database alternatives | Disabled and optional |

In this table, enabled optional services are selected by default but can be
excluded when an app is created. Required services cannot be excluded.

The PHP service also has a 20 GB `wp-content` volume and an optional SSH
derivative. Service links connect PHP to the selected database, shared storage,
mail service, and cache.

## Deploy this stack

Start from [Vanilla WordPress](https://github.com/wodby/wordpress-vanilla), or connect your own compatible source
repository.

Review service versions, storage, links, and optional components when creating
the application. The same stack can be reused across development, staging, and
production environments.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.

Validate the manifests with:

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/) and the [managed services index](https://github.com/wodby/services).
