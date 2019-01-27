## Merge Request Checklist

The merge requests should be configured such that **master** and **develop** branches are **protected** and even <span style="color:red">administrators</span> should not be able to merge without a merge request.

Before creating the merge request, the `developer` must ensure the following steps are addressed:

- Ensure the code, unit and automation tests execute successfully on Jenkins (and locally of course)
	- ideally each `developer` creates a Jenkins job and  configures the branch name accordingly
- Address the errors identified by static analysis tools (`lint`, `pmd`, `checkstyle` and `findbugs`)
- Based on the complexity of the code, your team may benefit from a code walk-through
- Create the merge request by following the pattern defined in [Merge Request Template](Merge_Request_Template.md)
- Update the status in JIRA


Additionally, a bunch of checks should run (in GitLab/GitHub) on a merge request when it is created and here are some of the integrations:

- [lgtm](https://github.com/lgtmco/lgtm) - specifies number of approvals required for a merge request and if self can approve
- [danger](https://github.com/danger/danger) - to enforce number of lines in a code change, leverage merge request template to ensure merge request title including JIRA number in the title and body are provided, code coverage limits, among others