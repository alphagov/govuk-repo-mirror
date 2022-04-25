# GOV.UK Repository Mirrorer

Ensures all repositories tagged with `govuk` in the alphagov organisation in GitHub.com are mirrored to AWS CodeCommit.

We use AWS CodeCommit so we can still [deploy in the event that GitHub.com is down][github-down]
. We also use the private repositories on AWS CodeCommit in order to fix any [security vulnerabilities without disclosing them][security-deployments].

This script will ensure every non-archived repository tagged with `govuk` on Github is mirrored to AWS CodeCommit. It will then synchronise all branches and the most recent tags, including removing branches that have been deleted in GitHub. In runs periodically in [Jenkins][jenkins].

**Warning:** If you need to work on a branch in private on AWS CodeCommit, this script must be prevented from running. If it runs and the branch you're working on does not exist in GitHub, it will be removed from AWS CodeCommit. The easiest way to prevent this is to untag the repository in GitHub.

## Usage

Required environment variables:

| Variable                   | Description                                                                                                         |
|:---------------------------|:--------------------------------------------------------------------------------------------------------------------|
| `GITHUB_ACCESS_TOKEN`      | A personal access token with `read:org` and `repo` scope                                                            |
| `AWS_CODECOMMIT_USER_ID`   | IAM user with privileges to assume the role in `ROLE_ARN`                                                           |
| `GIT_SSH_PRIVATE_KEY`      | A private key attached to the IAM user in `AWS_CODECOMMIT_USER_ID`                                                  |
| `ROLE_ARN`                 | IAM role with privileges to create, list and push to CodeCommit repositories                                        |

```shell
$ bundle install
$ ./mirror_repos
```

## Licence

[MIT License](LICENCE)

[github-down]: https://docs.publishing.service.gov.uk/manual/github-unavailable.html
[security-deployments]: https://docs.publishing.service.gov.uk/manual/deploy-fixes-for-a-security-vulnerability.html
[jenkins]: https://deploy.integration.publishing.service.gov.uk/job/Mirror_Github_Repositories/
