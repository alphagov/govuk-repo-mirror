# GOV.UK Repository Mirrorer

Ensures all repositories tagged with `govuk` in the alphagov organisation in GitHub.com are mirrored to AWS CodeCommit.

We use AWS CodeCommit so we can still [deploy in the event that GitHub.com is down][github_down]
. We also use the private repositories on AWS CodeCommit in order to fix any [security vulnerabilities without disclosing them][security_deployments].

This script will ensure every repository tagged with `govuk` on Github is mirrored to AWS CodeCommit. It will then synchronise all branches and tags, including removing branches that have been deleted in GitHub. It is designed to be run periodically in Jenkins.

**Warning:** If you need to work on a branch in private on AWS CodeCommit, this script must be prevented from running. If it runs and the branch you're working on does not exist in GitHub, it will be removed from AWS CodeCommit. The easiest way to prevent this is to untag the repository in GitHub.

## Usage

Required environment variables:

| Variable                   | Description                                                                                                         |
|:---------------------------|:--------------------------------------------------------------------------------------------------------------------|
| `GITHUB_ACCESS_TOKEN`      | A personal access token with `read:org` and `repo` scope                                                            |
| `AWS_ACCESS_KEY_ID`        | AWS credential for IAM user with privileges to create, list and push to CodeCommit repositories                     |
| `AWS_SECRET_ACCESS_KEY`    | AWS credential for IAM user with privileges to create, list and push to CodeCommit repositories                     |

```shell
$ bundle install
$ ./mirror_repos
```

## Licence

[MIT License](LICENCE)

[github_down]: https://docs.publishing.service.gov.uk/manual/github-unavailable.html
[security_deployments]: https://docs.publishing.service.gov.uk/manual/deploy-fixes-for-a-security-vulnerability.html
