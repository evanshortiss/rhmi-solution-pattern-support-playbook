# RHMI RHTE Lab Setup

_This repository is based on a template by Achim Christ._

## Usage

This is tested against RHMI 1.5 clusters and targets the User-Facing SSO. It
will execute the following tasks:
- create a realm for each of the 50 evals users on the cluster. The
evals users can then access the Realm via a URL like so _https://sso-user-sso.apps.$YOUR_CLUSTER_ID.open.redhat.com/auth/admin/evalsXX/console/index.html_.
- create a tenant in 3scale API Management for each of the 50 evals users. They will be able to access the accounts at https://evalsXX-admin.apps.$YOUR_CLUSTER_ID.open.redhat.com, with `evalsXX` as username and default password `Password1` (can be changed in `create-3scale-tenants/defaults/main.yml`).

**Note:** `evalsXX` is `evals01` to `evals50`.

Cluster Admin is required to get the SSO login password from the
`SSO_ADMIN_PASSWORD` environment variable from the `sso` deployment in the
namespace like so:

```bash
oc login $CLUSTER_URL -u admin

export SSO_ADMIN_PASSWORD=$(oc get dc/sso -n user-sso -o jsonpath='{.spec.template.spec.containers[0].env[?(@.name=="SSO_ADMIN_PASSWORD")].value}')
```

Then use the returned password like so:

```
# Replace the values here with your own
export YOUR_CLUSTER_ID=eshortis-83ef

ansible-playbook perform-setup.yml \
-e cluster_apps_host=apps.$YOUR_CLUSTER_ID.open.redhat.com \
-e sso_login_password=$SSO_LOGIN_PASSWORD
```