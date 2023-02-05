# RHODS-JIRA-Tools

A collection of tools for managing JIRA issues for the RHODS project

## How TO

Require python3.X and pipenv installed.

### get_release_issues.py

This script returns all the issues matching with  in a specific release.

#### Usage

```bash
python get_release_issues.py \
  --token-file /path/to/file/with/jira/personal/token \
  --release "The release to get all the issues from" # e.g. RHODS_1.6.0_GA
```

In combination with `awk` it can be used to format the input of the scripts
below.

```bash
python get_release_issues.py ... | awk '{ printf " --issue %s", $1 }'
```

### move_to_qa.py

This script handles transitioning a given JIRA issue to the "Ready for QA"
state. The thought is that this script can eventually be used as part of
a broader suite of automation for RHODS builds and handoff to QE.

#### Usage

```bash
python move_to_qa.py \
  --token-file /path/to/file/with/jira/personal/token \
  --build "the build version that the issue was resolved in" \
  --release "the RHODS target release that the issue was resolved in" \
  --issue "jira-issue-key" \
  --issue "some-other-jira-issue-key" \
  --issue "another-jira-issue-key"
```

#### Needed enhancements

This script is pretty barebones right now. Some enhancements that
would be nice:

1. Error checking in general
2. Validation to ensure that an issue has acks
3. Move common function (e.g token validation, init jira instance) into support module

### ack_checker.py

This script checks whether a given issue is fully acked. It does so by checking
the value of the `CDW Release` custom field. If the value is `+`, it is fully
acked, otherwise not.

This script could likely be used as part of a bot that will restrict merging a
PR to a RHODS repo if the associated JIRA issue is not fully acked.

#### Usage

```bash
python ack_checker.py \
  --token-file /path/to/file/with/jira/personal/token \
  --issue "jira-issue-key" \
  --issue "some-other-jira-issue-key" \
  --issue "another-jira-issue-key"
```
