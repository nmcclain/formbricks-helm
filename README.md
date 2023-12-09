# Formbricks Helm Chart for Kubernetes

[Formbricks](https://github.com/formbricks/formbricks) is an Open Source Survey Toolbox.
This is a Helm chart for deploying Formbricks on Kubernetes.

The easiest way to use Formbricks is their hosted service, with a generous free tier at [formbricks.com](https://formbricks.com/).

### Formbricks Features

- 📲 Create **in-product surveys** with our no-code editor with multiple question types.
- 📚 Choose from a variety of best-practice **templates**.
- 👩🏻 Launch and **target your surveys to specific user groups** without changing your application code.
- 🔗 Create shareable **link surveys**.
- 👨‍👩‍👦 Invite your team members to **collaborate** on your surveys.
- 🔌 Integrate Formbricks with **Slack, Posthog, Zapier, n8n and more**.
- 🔒 All **open source**, transparent and self-hostable.

### Built on Open Source (AGPLv3)

- 💻 [Typescript](https://www.typescriptlang.org/)
- 🚀 [Next.js](https://nextjs.org/)
- ⚛️ [React](https://reactjs.org/)
- 🎨 [TailwindCSS](https://tailwindcss.com/)
- 📚 [Prisma](https://prisma.io/)
- 🔒 [Auth.js](https://authjs.dev/)
- 🧘‍♂️ [Zod](https://zod.dev/)

## Install Chart

```
helm upgrade --install oci://ghcr.io/nmcclain/formbricks/formbricks --version 0.1.0
```

Source for this Helm Chart is located at: https://github.com/nmcclain/formbricks-helm

## Requirements
Before you start, ensure you have the following dependencies ready and working:

* Helm 3
* PostgreSQL Database
* Shared Filesystem for "uploads" if using multiple replicas
* Formbricks specific Helm values

At a minimum, you have to set the following in `values.yaml`:

```
formbricks:
  encryption_key: ""                     # REQUIRED: openssl rand -hex 32
  nextauth_secret: ""                    # REQUIRED: openssl rand -hex 32
  webapp_url: http://localhost:3000      # REQUIRED
  nextauth_url: "http://localhost:3000"  # REQUIRED
  database_url: "postgresql://postgres:postgres@postgres:5432/formbricks?schema=public"  # REQUIRED
```

Configuration documentation can be found here: https://formbricks.com/docs/self-hosting/docker#important-run-time-variables

## Releasing this Chart
You need to set `$GH_PAT` to a GitHub Personal Access Token with `read:packages`, `write:packages`, and `delete:packages` permission scopes.  Also set `$GH_USERNAME` to your GitHub user.

```
echo $GH_PAT | docker login ghcr.io -u $GH_USERNAME --password-stdin
echo $GH_PAT | helm registry login ghcr.io -u $GH_USERNAME --password-stdin
helm package formbricks
export CHART_VERSION=$(grep 'version:' ./formbricks/Chart.yaml | tail -n1 | awk '{ print $2}')
helm push formbricks-${CHART_VERSION}.tgz oci://ghcr.io/$GH_USERNAME/formbricks
```

## Support
* Open an issue if you run into trouble: https://github.com/nmcclain/formbricks-helm/issues
* Formbricks Docs: https://formbricks.com/docs/self-hosting
* Formbricks Docker image: https://hub.docker.com/r/formbricks/formbricks
* Formbricks Releases: https://github.com/formbricks/formbricks/releases
* Formbricks Discord: https://formbricks.com/discord
* I'm Ned - reach me at: https://www.nedmcclain.com/

## Contributing
Please do!
