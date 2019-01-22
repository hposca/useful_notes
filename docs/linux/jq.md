# Group AWS ECR images

With the help of https://github.com/stedolan/jq/issues/274#issuecomment-64925860

```bash
aws ecr list-images --repository-name ${repository_name} | jq '.[] | group_by(.imageDigest) | map({"digest": .[0].imageDigest, "tags": map(.imageTag)})'
```

This will display all the tags associated with the image digest.
