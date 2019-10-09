# RHMI RHTE Lab Setup

_This repository is based on a template by Achim Christ._

## Usage

This is tested against RHMI 1.5 clusters and targets the User-Facing SSO. It
will create a Realm for each of the 50 evals users on the cluster. The
evals users can then access the Realm via a URL like so _https://sso-user-sso.apps.$CLUSTER_ID.open.redhat.com/auth/admin/evals01/console/index.html_.

Cluster Admin is required to get the SSO login password from the
`SSO_ADMIN_PASSWORD` environment variable from the `sso` deployment in the
namespace like so:

```bash
oc login $CLUSTER_URL -u admin

oc get dc/sso -n user-sso -o json | jq '.spec.template.spec.containers[0].env'
```

Then use the returned password like so:

```
# Replace the values here with your own
export YOUR_CLUSTER_ID=eshortis-83ef
export SSO_LOGIN_PASSWORD=notactuallythepassword

ansible-playbook perform-setup.yml \
-e cluster_apps_host=apps.$YOUR_CLUSTER.redhat.com \
-e sso_login_password=$SSO_LOGIN_PASSWORD
```