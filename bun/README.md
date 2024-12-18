# Fission: NodeJS Environment

This is the NodeJS environment for Fission.

It's a Docker image containing a NodeJS runtime, along with a dynamic
loader.  A few common dependencies are included in the package.json
file.

Looking for ready-to-run examples? See the [NodeJS examples directory](../../examples/nodejs).

## Customizing this image

To add package dependencies, edit [package.json](./package.json) to add what you need, and rebuild this image (instructions below).

You also may want to customize what's available to the function in it's request context.
You can do this by editing [server.js](./server.js) (see the comment in that file about customizing request context).

## Rebuilding and pushing the image

You'll need access to a Docker registry to push the image: you can sign up for Docker hub at hub.docker.com, or use registries from gcr.io, quay.io, etc.
Let's assume you're using a docker hub account called USER.
Build and push the image to the the registry:

Building runtime image,

```console
docker build -t USER/bun-env --build-arg BUN_BASE_IMG=1.1.40-alpine -f Dockerfile .
docker build -t ghcr.io/danhtran94/bun-env --build-arg BUN_BASE_IMG=1.1.40-alpine -f Dockerfile .
docker push USER/bun-env
docker push ghcr.io/danhtran94/bun-env
```

Building builder image,

```console
cd builder && docker build -t USER/bun-builder --build-arg BUN_BASE_IMG=1.1.40-alpine -f Dockerfile .
cd builder && docker build -t ghcr.io/danhtran94/bun-builder --build-arg BUN_BASE_IMG=1.1.40-alpine -f Dockerfile .
docker push USER/bun-builder
docker push ghcr.io/danhtran94/bun-builder
```

## Using the image in fission

You can add this customized image to fission with "fission env create":

```console
fission env create --name node --image USER/bun-env
```

Or, if you already have an environment, you can update its image:

```console
fission env update --name node --image USER/bun-env
```

After this, fission functions that have the env parameter set to the
same environment name as this command will use this environment.