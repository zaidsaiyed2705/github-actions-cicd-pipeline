# CI/CD Deployment Pipeline using GitHub Actions

Automatically deploys a static website to an S3 bucket every time code is pushed to `main`,
using GitHub Actions as the CI/CD engine.

## Architecture

Developer pushes to `main` → GitHub Actions triggers → AWS credentials loaded from GitHub Secrets
→ `aws s3 sync` deploys `/site` to the S3 bucket → live website updates automatically

## AWS Services Used
- **S3** — static website hosting target
- **IAM** — dedicated deployer user (`github-actions-deployer`) with a least-privilege policy
  scoped to a single bucket (PutObject, DeleteObject, ListBucket only)

## Folder Structure
- `.github/workflows/deploy.yml` — the CI/CD pipeline definition
- `site/` — static website source files
- `screenshots/` — proof of working pipeline

## Steps Performed
1. Created an S3 bucket with static website hosting enabled and a public-read bucket policy.
2. Created a dedicated IAM user for GitHub Actions (not root, not personal credentials) with
   an inline least-privilege policy.
3. Generated an access key for that user and stored it in GitHub repository secrets.
4. Wrote a GitHub Actions workflow that triggers on push to `main`, configures AWS credentials,
   and syncs the `site/` folder to S3.
5. Pushed a change and verified the workflow ran successfully and the live site updated.

## Live Site
http://cicd-deploy-zaid-2026.s3-website.ap-south-1.amazonaws.com

## Screenshots
See `/screenshots` folder:
1. S3 static website hosting config
2. IAM user with least-privilege inline policy
3. GitHub repo secrets configured
4. Workflow run in progress
5. Workflow run completed successfully
6. Live website reflecting deployed content
7. Second deploy after a content change, confirming auto-deploy works

## Security Notes
- No root or personal AWS credentials used anywhere — dedicated IAM user with scoped permissions only.
- Secrets stored using GitHub encrypted secrets, never committed to the repo.

## Cleanup
S3 bucket and IAM access keys were removed after grading/review to avoid ongoing cost and exposure.
