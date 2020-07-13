# Only run GitHub Action on push to master

Spotted in [this Cloud Run example](https://github.com/GoogleCloudPlatform/github-actions/blob/20c294aabd5331f9f7b8a26e6075d41c31ce5e0d/example-workflows/cloud-run/.github/workflows/cloud-run.yml):

```yaml
name: Build and Deploy to Cloud Run

on:
  push:
    branches:
    - master
```

Useful if you don't want people opening pull requests against your repo that inadvertantly trigger a deploy action!

An alternative mechanism I've used is to gate the specific deploy steps in the action, [like this](https://github.com/zhangchunsheng/wechaty-grpc/blob/master/.github/workflows/go.yml#L35-L40).

```yaml
    # Only run the build if push was to master
    publish:
        if: github.event_name == 'push' && (github.ref == 'refs/heads/master' || startsWith(github.ref, 'refs/heads/v'))
        name: Publish
        needs: [build]
        runs-on: ubuntu-latest
        steps:
```
