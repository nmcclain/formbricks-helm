# Formbricks Helm Chart for Kubernetes

[Formbricks](https://github.com/formbricks/formbricks) is an Open Source Survey Toolbox.
This is a Helm chart for deploying Formbricks on Kubernetes.

The easiest way to use Formbricks is their hosted service, with a generous free tier at [formbricks.com](https://formbricks.com/).

Source for this Helm Chart is located at: https://github.com/nmcclain/formbricks-helm

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

## Install Formbricks

## Requirements
Before you start, ensure you have the following dependencies ready and working:

* Helm 3
* Existing PostgreSQL database
* Existing required k8s secrets - see below
* Formbricks specific Helm values

At a minimum, you have to set the following in `values.yaml`:

```
formbricks:
  webappUrl: http://localhost:3000
  nextauthUrl: http://localhost:3000
```

All options are defined in the [values.yaml](formbricks/values.yaml) file. Formbricks configuration documentation can be found here: https://formbricks.com/docs/self-hosting/docker#important-run-time-variables

### Create formbricks secret
* Create `formbricks` namespace: `kubectl create ns formbricks`
* Set your Postgres connection string, then execute the following command:

```
kubectl create secret -n formbricks generic formbricks-secrets \
	--from-literal=database_url='postgresql://formbricks:CHANGE_ME@postgres:5432/formbricks?schema=public' \
	--from-literal=nextauth_secret="`openssl rand -hex 32`" \
	--from-literal=encryption_key="`openssl rand -hex 32`" \
	--from-literal=cron_secret="`openssl rand -hex 32`"
```

### Install Chart

```
helm upgrade --install -n formbricks formbricks oci://ghcr.io/nmcclain/formbricks/formbricks --version 2.5.1
```

## Releasing this Chart
You need to set `$GH_PAT` to a GitHub Personal Access Token with `read:packages`, `write:packages`, and `delete:packages` permission scopes.  Also set `$GH_USERNAME` to your GitHub user.

```
* Update formbricks/Chart.yaml and README.md
echo $GH_PAT | docker login ghcr.io -u $GH_USERNAME --password-stdin
echo $GH_PAT | helm registry login ghcr.io -u $GH_USERNAME --password-stdin
helm package formbricks
export CHART_VERSION=$(grep 'version:' ./formbricks/Chart.yaml | tail -n1 | awk '{ print $2}')
git checkout -b "v${CHART_VERSION}"
git add formbricks/Chart.yaml README.md
git commit -m "formbricks version ${CHART_VERSION}"
git push
git tag ${CHART_VERSION}
git push origin ${CHART_VERSION}
create release in GitHub UI
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
