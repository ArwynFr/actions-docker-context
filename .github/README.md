# Github Action creating SSH Docker context

This action creates a [docker context](https://docs.docker.com/engine/context/working-with-contexts/), allowing access to a remote docker engine over SSH from your workflows. By default, you must refer to the context by its name with the `--context` switch, or you can set `use_context` to true in order to set that new context as the current docker context.

## Inputs

### docker_host
**Required**. URL to the remote docker engine in format `ssh://username@hostname.domain.tld`.

### context_name
**Required**. Name of the remote context on the local docker client.

### use_context
If set to `true`, the docker context will be set as the current docker context. Defaults to `false`.

### ssh_cert
Public key of the SSH server, in `known_hosts` format.
The public key is added to the current user's `known_hosts` file, and you can reuse previously added keys.
Using a Github secret is recommended.

### ssh_key
Private key of the SSH client.
The key is stored using ssh-agent, so you can reuse an existing private key instead.
Using a Github secret is recommended.

## Example

```yml
on:
  push: [master]

jobs:
  publish:
    runs-on: ubuntu-latest
    name: 'Publish on the development server'
    steps:

    - uses: arwynfr/actions-docker-context@v1
      with:
        docker_host: 'ssh://someone@example.domain.tld'
        context_name: 'dev-server'
        ssh_cert: ${{ secrets.SSH_CERT }}
        ssh_key: ${{ secrets.SSH_KEY }}

    - run: docker --context dev-server stack deploy --compose-file stack/docker-compose.yml my-stack
```
