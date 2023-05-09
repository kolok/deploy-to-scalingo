# Github action to deploy to scalingo

## Summary

This action can be used to deploy to Scalingo. Perhaps it can be used to deploy to other PaaS but I didn't test it

It executes 2 steps :

1. Install SSH private key to runner
1. add known_hosts (path of remote server) to known_hosts
1. push code to git repository to deploy it

## Set the scalingo ssh key

I advise you to create a specific ssh key to set to your scalingo account dedicated to deploy your code. To configure it, please follow this Scalingo doc page about set SSH key:

- [Guide for Linux](https://doc.scalingo.com/platform/getting-started/setup-ssh-linux)
- [Guide for MacOS](https://doc.scalingo.com/platform/getting-started/setup-ssh-macos)
- [Guide for Windows](https://doc.scalingo.com/platform/getting-started/setup-ssh-windows)

⚠️ Warning : don't set any passphrase to this key

Then add the public key to a Scalingo account which have the right to deploy your code (for example, your account)

And set the private key to github secret of you project : `Your repository > Settings > Secrets and variables > Actions > New repository secret`

Name it explicitely, for example SSH_PRIVATE_KEY and copy the private key content to the value.

## Usage

### inputs

- ssh-private-key : required, you need to set it to the github secret related to your SSH_PRIVATE_KEY
- app-name: required, name of your scalingo app
- known-host : optional, default is ssh.osc-fr1.scalingo.com

app-name and known_host are used to build the scalingo git repo using

```
git@${{ inputs.known-host }}:${{ inputs.app-name }}.git
```

set the action as follow :

```yaml
runs-on: ubuntu-latest
steps:
  - uses: actions/checkout@v3
  - uses: kolok/deploy-to-scalingo@v1
    with:
      ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
      app-name: app-name
      known-host: ssh.osc-fr1.scalingo.com
```
