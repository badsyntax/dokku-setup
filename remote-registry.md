# Using a remote docker registry

Use a remote registry for storing custom base docker images. 

## GitHub Container Registry

See also: https://docs.github.com/en/free-pro-team@latest/packages/managing-container-images-with-github-container-registry/pushing-and-pulling-docker-images

First create a new personal access token (`PAT`) with **only** `read:packages`, `write:packages` & `delete:packages` scopes. You MUST deselect the `repo` scope.

### Login to the registry

#### On the dokku server

On the dokku server, install the `dokku-registry` plugin and login to the GitHub container registry:

```bash
dokku plugin:install https://github.com/dokku/dokku-registry.git registry
dokku registry:login ghcr.io USERNAME "$PAT"
```

#### From local

Login to the registry from your local machine:

```bash
echo $PAT | docker login ghcr.io -u USERNAME --password-stdin
```

#### In GitHub Actions

Login to the registry within a GitHub Action (before building the docker image):

```yml
- name: Login to GitHub Container Registry
  run: |
    echo ${{ secrets.CONTAINER_REGISTRY_GITHUB_TOKEN }} | docker login ghcr.io -u ${{ secrets.USERNAME_GITHUB }} --password-stdin
```

## Publish an image

Build a local image and push it to the remote registry:

```bash
docker build -t ghcr.io/USERNAME/IMAGE_NAME:latest .
docker push ghcr.io/USERNAME/IMAGE_NAME:latest
```

## Reference remote image in Docker file

Create a Dockerfile that uses the remote image as a base image, for example:

```Dockerfile
FROM ghcr.io/USERNAME/IMAGE_NAME:latest
COPY index.html /usr/share/nginx/html/index.html
```

Add the dokku git remote:

```bash
git remote add dokku dokku@dokku.me:app-name
```

Deploy the app:

```bash
git push dokku
```

The image should be built by first pulling the base image from the remote registry.
