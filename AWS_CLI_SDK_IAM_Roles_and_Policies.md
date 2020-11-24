# AWS CLI, SDK, IAM Roles and Policies

- IAM -> users -> security credentials
- `aws configure`
- creates `~/.aws/credentials` and `.../config`
- DO NOT run `aws configure` on EC2
- use IAM Roles
- IAM -> Roles -> Create role -> services -> policies
- possible to create custom policies
- **Metadata**: reflection mechanism of EC2 instances
- http://169.254.169.254/latest/meta-data
- can retrieve IAM role, but not policy
- Any API that fails because of too many calls needs to be retried with exponential backoff