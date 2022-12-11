# Requirements

## PVCs

| Name | Purpose |
| ---- | ------- |
| puppet-serverdata | Puppet server local state/data directory |
| puppet-puppet | Puppet server configuration|
| puppet-code | Code deployed to nodes |
| puppet-ca | Puppet server & nodes certificates|
| puppet-puppetdb | Certs for PuppetDB <> Puppet server communication |

## Secrets

Encrypted using helm secrets plugin. Helm charts encode values to base64,
you don't need to encode them in `secrets.yaml` file.  
**puppet-code & puppet-hiera use the same SSH key**

r10k-code-creds:
 - id_rsa - SSH private key for puppet-code repository
 - known_hosts - github.com public key

r10k-hiera-creds:
 - id_rsa - SSH private key for puppet-hiera repository
 - known_hosts - github.com public key

puppet-db:
 - password - Password for database connection

# Installation

## Add helm chart repository

```
helm repo add puppet https://puppetlabs.github.io/puppetserver-helm-chart
helm repo update
```

## Install and upgrade

```
helm_chart_version="6.1.0"
helm secrets upgrade --install puppetserver puppet/puppetserver \
    -f values.yaml -f secrets.yaml -n puppet --version ${helm_chart_version}
```

