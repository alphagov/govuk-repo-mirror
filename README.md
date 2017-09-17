# GOV.UK Repository Mirrorer

Ensures all repositories tagged with "govuk" in the alphagov organisation in GitHub.com are mirrored to GitLab.com.

We use GitLab.com so we can still [deploy in the event that GitHub.com is down][github_down]
. We also use the private repositories at GitLab in order to fix any [security vulnerabilities without disclosing them][security_deployments].

This script will ensure a project exists in the `govuk` namespace in GitLab.com for every `govuk` GitHub repository. It will then synchronise all branches and tags, including removing branches that have been deleted in GitHub. It is designed to be run periodically in Jenkins.

**Warning:** If you need to work on a branch in private on GitLab.com, this script must be prevented from running. If it runs and the branch you're working on does not exist in GitHub, it will be removed from GitLab. The easiest way to prevent this is to untag the repository in GitHub.

## Usage

Required environment variables:

| Variable                   | Description                                                                                                         |
|:---------------------------|:--------------------------------------------------------------------------------------------------------------------|
| `GITHUB_ACCESS_TOKEN`      | A personal access token with `read:org` and `repo` scope                                                            |
| `GITLAB_API_PRIVATE_TOKEN` | A GitLab.com access token with privileges to create projects within the `govuk` namespace and push to those projects |

```shell
$ bundle install
$ ./mirror_repos
```

## Licence

[MIT License](LICENCE)

[github_down]: https://docs.publishing.service.gov.uk/manual/github-unavailable.html
[security_deployments]: https://docs.publishing.service.gov.uk/manual/deploy-fixes-for-a-security-vulnerability.html
