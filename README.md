# Nextcloud application stack for Kubernetes on Wodby

Deploy Nextcloud applications on Kubernetes with Wodby.

This repository defines the Wodby stack manifest and default service
composition for Nextcloud.

- [Nextcloud stack on Wodby](https://wodby.com/stacks/nextcloud)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- Nextcloud service (`nextcloud`)
- [PostgreSQL service](https://github.com/wodby/service-postgres)
- [Redis service](https://github.com/wodby/service-redis)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Nextcloud<br>`nextcloud` | required; main service; `data` volume 50 GB; links to PostgreSQL, Redis, and OpenSMTPD |
| PostgreSQL<br>`postgres` | required; `data` volume 20 GB |
| Redis<br>`redis` | required; used for cache and transactional file locking |
| OpenSMTPD<br>`opensmtpd` | optional; enabled by default for outgoing email |

The Nextcloud service uses the official Nextcloud Apache image and the upstream
Nextcloud Helm chart. Wodby manages its route, TLS, PostgreSQL and Redis links,
and persistent volume.

Nextcloud can optionally use Amazon S3, Cloudflare R2, or Backblaze B2 as its
primary file-content store through a Wodby storage integration. Configure a
dedicated empty bucket and attach the integration before the application's
first deployment. S3 primary storage must not be enabled later on an existing
installation because it does not migrate files already stored on the volume.

Enabled optional services are selected by default but can be excluded when an
app is created. Required services cannot be excluded.

## Deploy this stack

Create an app from the Nextcloud stack, then provide the initial administrator
username, for example `admin`, and a strong password.

Review the selected Nextcloud version, storage sizes, and optional SMTP service
when creating the application. The same stack can be reused across development,
staging, and production environments.

Back up the Nextcloud data volume together with PostgreSQL and, when enabled,
the S3 bucket. When upgrading Nextcloud, move one major version at a time.

## Maintain a custom version

1. Fork this repository.
2. Edit `stack.yml`.
3. Import the repository as a
   [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target.
Stack-local names and referenced service names are distinct identifiers.

Validate the manifest with:

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

See the [stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)
and the [managed stacks index](https://github.com/wodby/stacks).
