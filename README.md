# WordPress stack for Wodby

Deploy WordPress on Kubernetes with [Wodby](https://wodby.com). This repository
contains the `stack.yml` manifest used by the public WordPress stack in the
Wodby catalog.

- [WordPress stack in the Wodby catalog](https://wodby.com/stacks/wordpress)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

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

## Use this stack

The simplest path is to add the
[public WordPress stack](https://wodby.com/stacks/wordpress) from the Wodby
catalog. Review the enabled services, versions, storage sizes, and other
defaults when creating your app.

To maintain your own version of the stack:

1. Fork this repository.
2. Edit [`stack.yml`](stack.yml).
3. Import the repository as a
   [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

Wodby imports the manifest from the selected Git branch or tag and creates a
new stack revision when the Git-backed stack is updated.

## Customize the manifest

Common changes include:

- selecting another default PHP version;
- changing persistent volume sizes;
- enabling or disabling optional services;
- replacing Mailpit with OpenSMTPD for outbound email; and
- replacing the in-cluster MariaDB service with a cloud database service.

When changing a linked service, update the corresponding `services[].links`
target as well. For example, point PHP's `db` link at the cloud database service
before disabling the in-cluster `mariadb` service.

Validate a customized manifest with the Wodby CLI before importing it:

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)
for every supported field and the [managed services
index](https://github.com/wodby/services) for available service references.
