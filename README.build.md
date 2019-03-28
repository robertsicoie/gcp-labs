# Cloud Build
Create and run Cloud Build steps
---
Create a `Dockerfile`:
```bash
cat <<EOF > Dockerfile 
FROM alpine 
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/bin/sh", "/entrypoint.sh"]
EOF
```

Create `entrypoint.sh`:
```bash
cat <<EOF > entrypoint.sh
#!/bin/sh
echo Hello world! 
EOF
```

Set project id:
```bash
export PROJECT_ID=`gcloud config get-value project`
```

Create a file name cloudbuild.yaml:
```bash
cat <<EOF > cloudbuild.yaml
steps:
- name: 'ubuntu'
  args: ['echo', 'Salut!']
- name: 'gcr.io/cloud-builders/docker'
  args: ['build', '-t', 'gcr.io/$PROJECT_ID/hello-world', '.']

images:
- 'gcr.io/$PROJECT_ID/hello-world'
EOF
```

Run Cloud Build to create the image:
```bash
gcloud builds submit --config cloudbuild.yaml .
```

To retrieve the image run:
```bash
gcloud docker -- pull gcr.io/$PROJECT_ID/hello-world
```

To run it:
```bash
gcloud docker -- run gcr.io/$PROJECT_ID/hello-world
```

Cleanup
---
List images:
```bash
gcloud container images list
```

Remove the image:
```bash
gcloud container images delete --quiet gcr.io/$PROJECT_ID/hello-world
``` 
In case you have create multiple images without tagging them you will have to specify the digest and remove each image individually.