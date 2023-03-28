# POC: multi apps custom release workflow with release branches

A POC for a custom release workflow involving multiple applications running on the same codebase.

## Overview of the custom CI workflow:

- Release on its own branch (new version)
- Multiple projects with multiple variables within a single code base
- Use Vercel GitHub integration with custom workflow instead of Vercel CLI

## Current problems:

- Need to manually remove and re-add the production domain
- after re-adding prod domain still manual process to push empty commit
- no instant rollbacks

## Example new workflow:

- work on feature
- merge feature into develop branch `main`
- cut off version `v1.0.0`
- push code to `main`
- vercel auto deploy to all projects
- manual alias prod domain to both projects
- push new commit or merge another feature (cherry pick)
- manual alias prod domain to only one project

## Testing:

- push new commits on all branches should not affect prod domain
- merging PRs with new features should not affect prod domain
- cherry-picking on any branch should not affect prod domain
- there should be no Preview comments on prod
- test instant manual rollbacks (using alias with previous deployments)

## Q&A:

Q: How to check what commit / deployment is currently running on prod

- `vercel inspect {prod_url}`

Q: Where to get deployment URL after changes?

1. Vercel UI -> Deployments - filter branch (select version)
2. last deployments `vercel ls {project_name}`
3. GH action output

Q: How to check deployment URL by domain URL?

- `vercel inspect {prod_url}`
- alternatively check list of aliases `vercel alias ls`

Q: how to manually promote to prod deployment

- `vercel alias set deployment_url prod_domain`

Q: How to manually rollback?

- locate deployment url to rollback using Vercel UI or Vercel CLI
- `vercel alias set deployment_url prod_domain`
