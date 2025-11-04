# GCP Configuration


1. Enable APIs
- IAM service account credentials API
- Secret manager API (optional if SA needs access to secrets)
- Any other required APIs

2. Create service account with the relevant role(s)

3. In IAM & Admin → Workload Identity Federation
- Create new pool

4. Inside your new pool, create new Provider
- Choose OIDC
- Issuer (URL): https://token.actions.githubusercontent.com
- Allowed audiences: https://github.com/{owner}/{repo}

Attribute mapping:

- google.subject => assertion.sub

- attribute.repository => assertion.repository

- attribute.ref => assertion.ref

Attribute conditions (optional to limit which branch can authtenticate)

- attribute.repository == "{owner}/{repo}" && attribute.ref == "refs/heads/main"

5. Grant access to service account

- principalSet://iam.googleapis.com/projects/{project_number}/locations/global/workloadIdentityPools/{github-pool}/attribute.repository/{owner}/{repo}

- Assign role: Workload Identity User (roles/iam.workloadIdentityUser)

# Github Action Secrets (GCP configs)

### GCP_WIF_PROVIDER

`projects/{project_id}/locations/global/workloadIdentityPools/{github-pool-id}/providers/{github-provider-id}`
 </br> </br>

### GCP_SERVICE_ACCOUNT

`{gcp_service_account_email}`
 </br> </br>

### GCP_WIF_AUDIENCE

`https://github.com/{owner}/{repo}`
 </br> </br>

### GCP_SECRET

`projects/{project_id}/secrets/{secret_name}:latest`
