# Ansibot Help

Making progress in resolving issues for modules depends upon your interaction! Please be sure to respond to requests or additional information as needed. If at anytime you think this bot is misbehaving, please leave a comment containing the keyword **bot_broken** and an Ansible staff member will intervene.

#### Table of contents

* [Commands](#commands)
* [For issue submitters](#for-issue-submitters)
* [For pull request submitters](#for-pull-request-submitters)
* [When will your pull request be merged?](#when-will-your-pull-request-be-merged)
  * [New Modules](#new-modules)
  * [Existing Modules](#existing-modules)
  * [Non-module changes](#non-module-changes)
* [For community maintainers](#for-community-maintainers)
* [For anyone else](#for-anyone-else)
* [When to use label commands](#when-to-use-label-commands)

## Commands

To streamline the maintenance process, we've added some commands to the ansibot that you can use to help direct the work flow. Using the automation is simply a matter of adding one of the following commands in your comments:

Command | Scope | Allowed | Description
--- | --- | --- | ---
**bot_broken** | issues pull requests | anyone | Use this command if you think the bot is misbehaving, and an Ansible staff member will investigate.
**bot_skip** | issues pull requests | staff | Ansible staff members use this to have the bot skip triaging an issue.
**bot_status** | pull requests | submitters maintainers | Use this command if you would like the bot to comment with some helpful metadata about the issue.
**needs_info** | issues pull requests | maintainers past committers | Use this command if you need more information from the submitter. We will notify the submitter and apply the `needs_info` label.
**!needs_info** | issues pull requests | maintainers past committers | If you do not need any more information and just need time to work the issue, leave a comment that contains the command `!needs_info` and the `needs_info` label will be replaced with `waiting_on_maintainer`.
**needs_revision** | pull requests | maintainers | Use this command if you would like the submitter to make changes.
**!needs_revision** | pull requests | maintainers | If you want to clear the `needs_revision` label, use this command.
**needs_rebase** | pull requests | maintainers | Use this command if the submitters branch is out of date. The bot should automatically apply this label, so you may never need to use it.
**!needs_rebase** | pull requests | maintainers | Clear the `needs_rebase` label.
**notabug** | issues | maintainers | If you believe this is not a bug, please leave a comment stating `notabug`, along with any additional information as to why it is not, and we will close this issue.
**bug_resolved** | issues | maintainers | If you believe this issue is resolved, please leave a comment stating `bug_resolved`, and we will close this issue.
**resolved_by_pr** | issues | maintainers | If you believe this issue has been resolved by a pull request, please leave a comment stating `resolved_by_pr` followed by the pull request number.
**wontfix** | issues | maintainers | If this is a bug that you can't or won't fix, please leave a comment including the word `wontfix`, along with an explanation for why it won't be fixed.
**needs_contributor** | issues | maintainers | If this bug or feature request is something that you want implemented but do not have the time or expertise to do, comment with `needs_contributor`, and the issue will be put into a `waiting_on_contributor` state.
**duplicate_of** | issues | maintainers | If this bug or feature request is a duplicate of another issue, comment with `duplicate_of` followed by the issue number that it duplicates, and the issue will be closed.
**close_me** | issues | maintainers | If the issue can be closed for a reason you will specify in the comment, use this command.
**ready_for_review** | pull requests | submitters | If you are finished making commits to your pull request or have made changes due to a request, please use this command to trigger a review from the maintainer(s).
**shipit** | pull requests | maintainers | If you approve of the code in this pull request, use this command to have it merged. Note that `Approve` pull request status is ignored, this command must be used in order to approve the pull request.
**+label** | issues pull requests | staff maintainers | Add a whitelisted label. See [When to use label commands](#when-to-use-label-commands).
**-label** | issues pull requests | staff maintainers | Remove a whitelisted label. See [When to use label commands](#when-to-use-label-commands).

## For issue submitters
Please note that if you have a question about how to use this feature or module with Ansible, that's probably something you should ask on the [ansible-project](https://groups.google.com/forum/#!forum/ansible-project) mailing list, rather than submitting a bug report. For more details, please see http://docs.ansible.com/ansible/community.html#i-ve-got-a-question.

If the feature/module maintainer or ansibot needs further information, please respond to the request, so that you can help the devs to help you!

The bot requires a minimal subset of information from the issue template:
* issue type
* component name
* ansible version
* summary

If any of those items are missing or empty, ansibot will keep the issue in a `needs_info` state until the data is provided in the issue's description. The bot is expecting an issue description styled after the default issue template, so please use that whenever possible.

Expect the bot to do a few things:

1. Add common labels such as `needs_triage`, `bug_report`, `feature_idea`, etc.

   These labels are determined by templated data in the description. Please fill out the templates as accurately as possible so that the appropriate labels are used.

   `needs_triage` will be added if your issue is being labeled for the first time. We (ansible staff and maintainers) use this label to find issues that need a human first touch. We'll remove it once we've given the issue a quick look for any labeling problems or missing data.

2. Notify and assign the maintainer(s) of the relevant file(s) or module(s).

   Notifications will happen via a comment with the `@NAME` syntax. If you know of other interested parties, feel free to ping them in a comment or in your issue description.

If you are not sure who the issue is waiting on, please use the **bot_status** command.

## For pull request submitters
Expect the bot to do a few things:

1. All of the items described in the for [issue submitters](#for-issue-submitters) section.

2. Add labels indicating the status of the pull request.

  * `needs_rebase` - Your pull request is out of sync with ansible/ansible's devel branch. Please review the [rebase guide](http://docs.ansible.com/ansible/dev_guide/developing_rebasing.html) for further information.
  * `needs_revision` - Either your pull request fails continuous integration tests or a maintainer has requested a review/revision of the code. This label can be cleared by fixing any failed tests or by commenting `ready_for_review`
  * `community_review` - The bot is waiting for two or more module maintainers, maintainers of module in the same namespace, or core team members, to use the `shipit` command on this pull request.
  * `contributor_review` - The bot is waiting for anyone on the Ansible organization to use the shipit command on this pull request.
  * `core_review` - The bot is waiting for anyone on the Ansible core-team to use the `shipit` command on this pull request.
  * `shipit` - The shipit count has hit the minimum threshold and the pull request is ready for manual merge or automerge.

Please prefix your pull request's title with **WIP** if you are not yet finished making changes. This will tell the bot to ignore the `needs_rebase` and `shipit` workflows until you remove it from the title.

If you are finished committing to your pull request or have made changes due to a request, please use the **ready_for_review** command.

If you are not sure who the pull request is waiting on, please use the **bot_status** command.

### When will your pull request be merged?

`Approve` pull request status is ignored, `shipit` command must be used in order to approve a pull request.

#### New Modules

New modules require two **shipits** from anyone in the community before the bot will label it `shipit`. At that point, the module will be merged once a member of the Ansible organization has reviewed it and decided to include it.

#### Existing Modules

Module's have metadata with a `supported_by` field per the [metadata proposal](https://github.com/ansible/proposals/issues/30). The possible values of `supported_by` are:
* **unmaintained**: no community members are responsible for this module, so changes will have to be reviewed by the core team until someone volunteers to maintain it. See "core".
* **core**: Members of the Ansible organization typically do all the maintainence on this module, so only they can approve changes. Expect reviews to take longer than most other modules because of the volume the core team has on a daily basis.
* **curated**: These modules are developed and maintained by the community, but the Ansible core team needs to approve changes. Once two or more module maintainers, maintainers of a module in the same namespace, or core team members give `shipit`, the core team will be alerted to review.
* **community**: These modules are also developed, maintained and supported by the community. If you are a module maintainer, a maintainer of a module in the same namespace, or a core team member use the `shipit` command to approve the pull request. The bot will wait for two shipits from module maintainers, maintainers of a module in the same namespace, or core team members, then automerge.

NOTE: If you have **changes to other files in the pull request**, the `supported_by` property is ignored because the Ansible core team **must** approve those changes.

#### Non-module changes

The ansible core team approves these pull requests and it may take some time for them to get to your request.

## For community maintainers
Thanks in advance for taking a look at issues and pull requests and for your ongoing maintainince. If you are unable to troubleshoot or review this issue/pull request with the information provided, please ping the submitter of the issue in a comment to let them know.

## For anyone else
Reactions help us determine how many people are interested in a pull request or have run across a similar bug. Please leave a +1 [reaction](https://github.com/blog/2119-add-reactions-to-pull-requests-issues-and-comments) (:+1:) if that applies to you. Any additional details you can provide, such as your usecase, environment, steps to reproduce, or workarounds you have found, can help out with resolving issues or getting pull requests merged.

## When to use label commands

The `+label` and `-label` commands are restricted to a subset of available labels and are not meant to replace the other bot commands.

* needs_triage - a human being still needs to validate the issue is properly labeled and has all the information required.
* module - classifies the issue as a module related issue.
* affects_X.X - indicates that the issue is relevant to a particular ansible major.minor version.
* easyfix - a maintainer has decided that this is a trivial fix that new contributors would be able to tackle.
* c:... - these labels categorize issues or pull requests by their relevant source code files.

To use the commands, please type the the command and label on one line each in a comment.
Example:
```
-label needs_triage
+label cloud
+label gce
```
