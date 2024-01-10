# lemmy-docker-multiarch

[![Build Multiarch Images](https://github.com/ubergeek77/lemmy-docker-multiarch/actions/workflows/build-multiarch.yml/badge.svg?branch=main)](https://github.com/ubergeek77/lemmy-docker-multiarch/actions/workflows/build-multiarch.yml)

A build repository for multiarch Docker images for Lemmy.

Builds [`LemmyNet/lemmy`](https://github.com/LemmyNet/lemmy/) and [`LemmyNet/lemmy-ui`](https://github.com/LemmyNet/lemmy-ui/) for:

- x64 (`amd64`)
- ARM (`arm/v7`)
- ARM64 (`arm64`)

These images were originally created when Lemmy was at version `0.17.x`, because Lemmy only supported `amd64`. Lemmy started officially providing `arm64` images in version `0.18.0` by including some of the changes from this repository, but as of `0.19.2`, `arm/v7` images are still not officially provided by Lemmy. Since some users still want to run Lemmy on ARM32 hardware, I will continue to update these images until the Lemmy project officially builds images for `arm/v7`.

My images are as close to upstream as possible; I only change the Dockerfiles so that they will build on virtually any host, without needing cross-compilation or platform-specific steps. The Dockerfiles I use, and the workflow I use to compile these images, are all open source here. [You can see the logs of previous runs on the Actions tab](https://github.com/ubergeek77/lemmy-docker-multiarch/actions/workflows/build-multiarch.yml).

When [`LemmyNet/lemmy`](https://github.com/LemmyNet/lemmy/) or [`LemmyNet/lemmy-ui`](https://github.com/LemmyNet/lemmy-ui/) have new tags, my workflow will automatically be launched and those new tags will be built.

These images are primarily here so they can be used in my [Lemmy-Easy-Deploy](https://github.com/ubergeek77/Lemmy-Easy-Deploy) project. However, you may use these images manually if you like. They are drop-in replacements for the official Lemmy images.

I don't tag `latest`, so you will need to specify a tag to pull. For example, to use `0.19.2`:

```
ghcr.io/ubergeek77/lemmy:0.19.2
ghcr.io/ubergeek77/lemmy-ui:0.19.2
```

I also build `rc` tags. In general, I will have images for any stable or `rc` tag of the official Lemmy repositories. To see the full list of tags I've built, check the Packages section on this repo, or go to the images directly:

- https://github.com/ubergeek77/lemmy-docker-multiarch/pkgs/container/lemmy
- https://github.com/ubergeek77/lemmy-docker-multiarch/pkgs/container/lemmy-ui
