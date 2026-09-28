# docker-images

This repository contains files for building [Docker](https://www.docker.com/) images. All images should be available on [public.ecr.aws/juwaiiqi](https://gallery.ecr.aws/juwaiiqi/).

Publishing is manual (no CI builds/pushes these images) - check the tag table for each image below before rebuilding and pushing over an existing tag.

## amazonlinux-laravel-php tags

| Tag | PHP | Built from |
| --- | --- | --- |
| `1.0`, `buildx-latest` | 7.3 | commit `12fbd50` - do **not** rebuild from current master, master is on PHP 7.4 |
| `php74` (pending publish) | 7.4 | current master |

