# skopeo-how-to
"How to" use Skopeo for OCI image relocation in airgapped scenarios

Source: https://github.com/containers/skopeo

## Dev setup
- Install Skopeo: https://github.com/containers/skopeo/blob/main/install.md
- Run a local registry: `docker run -d -p 5001:5000 --restart always --name registry registry:2`
- Create an "images" directory

## How to...

### Locally save an image
```bash
skopeo copy docker://amd64/nginx:1.22.0 dir:images/nginx

skopeo copy docker://amd64/bash:5.1.16 dir:images/bash
```

### Get the repository list in the local registry
```bash
curl localhost:5001/v2/_catalog
```
When doing this exercisize for the first time, it should be empty

### Sync local directory (containing multiple images) with a local repository
```bash
skopeo sync --dest-tls-verify=false --src dir --dest docker images/ localhost:5001
```

### Verify that the local repository images are correctly tagged
Ensure the image doesn't exist already:
```bash
docker image remove localhost:5001/nginx
docker image remove localhost:5001/bash
```

Pull the newly synced images from the local repository
```bash
docker pull localhost:5001/nginx
docker pull localhost:5001/bash
```
