# The Human Cell Atlas Metadata Update Process SOP - v2.0

# Table of Contents
- [Introduction](#introduction)
- [General steps of the update process](#general-steps-of-the-update-process)
- [Schema update acceptance process](#schema-update-acceptance-process)
- [Specific steps for contributing changes](#specific-steps-for-contributing-changes)

## Introduction

Information about the process by which the HCA metadata schema will evolve is outlined here. Version 1.0 of this document can be found in the [HCA metadata lifecycle and versioning](https://docs.google.com/document/d/1eUVpYDLu2AxmxRw2ZUMM-jpKNxQudJbznNyNRp35nLc/edit#heading=h.6p3dwsx7c3hb) document. The v2.0 document here includes general update process steps, specific instructions for making suggested changes via a pull request, and the acceptance process and criteria for change acceptance. The HCA schema design principles, the semantics and process for versioning and updating these schema, and discussion of the detailed format and syntax of the metadata schema and their instantiation can be found in the complementary document [Metadata schema structure specification](https://docs.google.com/document/d/1pxQj7BfM8HHgD4ilm4dlvZuZATfJkNC5s_-TUoA4lYA/edit?ts=59b16455). These documents should be viewable by everyone. Please contact us if you do not have access to view.

## General steps of the update process

1. **Receive feedback on metadata standard.** Anyone can be a contributor and suggest changes to the metadata. Contributors can suggest changes via three main routes:
    1. Email the DCP helpdesk data-help@humancellaltas.org
    1. Create an issue on the metadata-schema GitHub repo
    1. Make a pull request against the develop branch of the metadata-schema GitHub repo

    To request an update to an existing schema, the contributor needs to supply four pieces of information. 

    - Which schema needs be changed?
    - What field(s) in that schema need to be changed?
    - What should the change be?
    - Why is the change requested?

    To request a new module schema, the feedback needs to contain three pieces of information.

    - What is the name of the new module schema?
    - What fields should it contain (in JSON schema or spreadsheet/tab form)?
    - Why it the new module needed? Specifically, why does the current metadata schema not meet the contributor's need?
    
    To request any new fields or changes to fields, the following information needs to be supplied:
    
    - Field name (required)
    - Field description (required)
    - Whether the field should be optional or required (required)
    - An example valid value for this field (optional)
    - If this field needs a controlled vocabulary or ontology, what those values should be (optional)

    If the suggested update comes in the form of the pull request, the required information should be self-evident between the JSON schema itself and the pull request message. If this isn’t clear either from the pull request or the message sent through email or git, then the assigned committer will seek clarification before moving onto step 3.

2. **Assign a committer to review the suggestion.** A person with commit rights on the metadata-standards repo should be assigned to the review process for each suggestions. This person will be responsible for reviewing the suggestion and ensuring upstream and downstream consequences are noted. They will contact the appropriate HCA teams and making the general community announcements ensuring the whole community has opportunity to give input. 

3. **Make pull request (if one doesn’t exist already).** If the feedback didn’t come in the form of the pull request, then one needs to be created. If the contributor has a GitHub account, they should be encouraged to make a pull request for their proposed change. ,If that is not possible, the committer who is handling the feedback should translate it into a JSON schema update and create the pull request. If this happens, another committer should be added to review and merge the pull request.

4. **Annotate the pull request with suitable labels and run integration tests.** The assigned reviewer should classify the suggested change by the type of change it is from a versioning perspective (patch, minor, or major) and the stability category of the module being changed (high, medium, or low). These labels can be setup in the GitHub repo so which pull requests have been flagged with which category are easy to see and filter. 

    The reviewer should also ensure the integration tests have been run. Minor and patch version changes should not cause the tests to fail. A major version change may cause test failures, especially when in a high stability module and this should help the reviewer to select the appropriate people to assist with the review.

    We aim for steps 1 to 4 to be completed less than 48 hours from the initial request being received.

5. **Notify community about the change request.** The pull request should be posted to the #hca-metadata Slack channel and the metadata-wg mailing list so everyone can have an opportunity to comment over the time frame specified by the combination of stability level and version change.

    The committer leading this effort should ensure appropriate developers from any system which could be impacted by the change are watching the thread on the pull request.

6. **Start the update acceptance process.** The assigned committer now starts the clock on the update acceptance process (see below). They should ensure the community is reminded about the proposed change at least once during the review timeframe. They also need to ensure any other assigned reviewer actively accepts the change and/or provides feedback on the change. 

7. **Accept or reject the change.** Once the review period is over, if the change is accepted into the repository, the pull request will be merged. The pull request merge should trigger automated updates to the documentation and propagation of the new schemas through the ingest system. The precise timing of the new schema version into production will depend on any software consequences for the change. Any patch or minor version change should be usable very quickly but major version changes might take longer depending on how long it takes any affected piece of software to be updated to function with the new schema.

8. **Announce accepted change to community**. The change will be announced both on #hca-metadata Slack channel and the metadata-wg mailing list by the committer. 

## Schema update acceptance process

There are three types of update acceptance verification: full, medium, and light. These three levels aim to facilitate agility for simple changes with few downstream dependencies but ensure that appropriate consideration is required for changes which are likely to trigger software updates. The table below describes which acceptance process is used in which circumstance.

All reviews should consider what need is met by the requested change and what the impact of the change is on the existing systems, ensuring that updates which require major version changes also bring value to the DCP as a whole.

The *review timeframe* should start when the committer responsible for the suggestion has notified the #hca-metadata slack channel and the metadata-wg email list about the pull request. Weekend days do not count for the review timeframe.

The *minimum reviewer number* is the minimum number of people who need to have seen and agreed to the change before the pull request can be accepted. This agreement must be active and result in at least +1 on the pull request thread. If a DCP team member or metadata-schema repo committer are the person to suggest a change, they should not be counted as one of the reviewers needed for agreement.

Any *negative marks* (-1) against a proposal that can not be resolved in the allotted timeframe will need to extend the review process. The full details of this are described below. 

If during this process there is any disagreement about the best approach, the assigned committer should work with the contributor and other concerned third parties to find an acceptable solution for all concerned. If consensus still cannot be reached, a vote between the committers for the metadata schema repo should decide the right solution.

### Full acceptance

**Review timeframe**: 10 working days

**Minimum reviewer number**: 3 (a committer, and two relevant third parties)

Full acceptance is used when making major version changes to high stability modules. This means the module being changed is likely to trigger a new software release for a DCP service. The DCP service which is most likely to be affected by any metadata update would be the Ingest Infrastructure. This should be a rare occurrence. The DCP metadata and code should have a clear separation of concerns and the dependencies between the two should be minimal. That being said, the ingest infrastructure, in particular, needs some knowledge of the metadata structure in order to enable ID fields to be assigned and to allow the system to know the relationships between the submitted entities. Any module which contains these fields must be considered high stability.

This process should take 10 working days. The assigned committer should identify at least two developers from the DCP team to review the change and comment as well as themselves. If the contributor making the suggestion was a developer from a DCP team, the two reviewers should be other DCP developers. 

### Medium acceptance

**Review timeframe**: 120 hours (5 working days)

**Minimum reviewer number**: 2 (a committer and one relevant third party)

Medium acceptance is the process used for major version changes to medium stability modules and minor version changes to both high and medium stability modules. 

Generally this is a suitable acceptance process for changes which don’t require DCP software updates but are likely to affect how the metadata can be queried from a scientific perspective. It is expected for these types of updates to happen more frequently than those which require full acceptance as they will concerns fields with scientific meaning. New experiments and changes in technology may require new fields or changes to existing fields.

The process should take 5 working days. The assigned committer should identify at least one interested third party from the secondary and tertiary analysis groups to provide review. As before if it the contributor is a member of the secondary or tertiary analysis group, another person should be selected as the reviewer.

### Light acceptance

**Review timeframe**: 72 hours (3 working days)

**Minimum reviewer number**: 1 (a committer)

Light acceptance is the process used for all patch versions, regardless of stability and for all changes to low stability modules.

Light acceptance is lightweight process which allows rapid iteration for new technologies where the requirements are not precisely defined and the processes are changing quickly.  Downstream users of this data should be aware that the metadata is changing rapidly and understand any dependencies they create on specific structures will be broken in future.

The process should take 3 working days. The assigned committer does not need to recruit an additional reviewer though should ensure that as many people as possible have seen the suggestion via slack messages or email.

| | Major version | Minor Version  | Patch version |
|:-|:-|:-|:-|
| High Stability | Full acceptance | Medium acceptance | Light acceptance |
| Medium Stability | Medium acceptance | Medium acceptance | Light acceptance |
| Low Stability | Light acceptance | Light acceptance | Light acceptance |

> Table 1: Which update acceptance process to use for which category of suggested change.

### How to manage negative marks against a suggested change.
Hopefully negative marks or a lack of consensus will be very rare occurrence but this process does contain the possibility of disagreement on the best solution. 

For proposed changes where consensus looks unlikely, as the review deadline looms (~ 1 day before if possible), the assigned committer should schedule a phone call to discuss the issue with the interested parties. If no consensus can be reached, the process should be extended by 5 working days for more interaction. At the end of these 5 days, if no consensus has been reached, the two possible solutions should be presented to the committers and a vote should be called with the committers to the metadata-schemas repo, the solution with most votes being taken forward.

## Specific steps for contributing changes

This section outlines steps for contributors to suggest changes to the metadata schema via pull requests.

1. **Clone** the metadata-schema repository into your local environment.

    `git clone git@github.com:HumanCellAtlas/metadata-schema`

1. **Navigate** to the metadata-schema repo.

    `cd metadata-schema`
    
1. **Switch** to the develop branch. All suggested changes should be based on the current develop branch.

    `git checkout develop`

1. **Make** and **switch** to a new working branch from develop. Name the branch following the convention: `username-brief_desc_of_branch_scope`

    `git checkout -b malloryfreeberg-new_mouse_module`

1. **Make** changes locally to the new working branch. After making changes, it is important to run the `src/schemas_are_valid_json.py` and `src/json_examples_validate_against_schema.py` scripts (which are run by Travis CI). The first script checks whether each .json file in the `json_schema` folder is valid JSON format. The second script attempts to validate example JSON files in the `schema_test_files` directory against their corresponding schemas. Some of the JSON files are meant to fail (e.g. they are lacking required fields) and as their failure is expected behavior, the script should exit with status 0. Ensure both scripts exit with status 0 (you should see `Process finished with exit code 0` printed to the terminal) before committing changes. If either test fails, you will have to debug and fix the errors in the changes you made.

1. **Stage** and **commit** your changes to the working branch often. We recommend committing after making a few logically grouped changes to help track changes and to increase granularity for rollbacks (if needed). Use helpful/short messages in commit statements. If the commit specifically fixes/addresses a current GitHub issue, you can add the phrase "Fixes #000" to the commit statement, replacing "000" with the number of the issue. This phrase is handy because when the changes are merged into the master branch, it automatically closes the issue indicated.

    `git add <changed files>`
    
    `git commit -m "Helpful commit message here. Fixes #000."`
    
    Example commit message: "Created new mouse module with mouse-specific fields. Fixes #142."

1. **Push** the committed changes to the working branch.

    `git push origin malloryfreeberg-new_mouse_module`

1. **Continue** to make, stage, and commit changes to the working branch - ensuring that the two Travis CI scripts pass - until you have completed and pushed all the changes within the scope of your new branch. In the GitHub UI, **create** a pull request (PR) against the `develop` branch. In the comment section of the PR, write a general description of the changes followed by a bulleted list of the specific changes made in the files. 

1. **Request** a reviewer to send notifications that a PR needs to be reviewed and merged. **Do not merge your own PR.** If the changes are ultimately approved, the old branch will be deleted unless otherwise specified by the contributor.

## Observed guidelines

- *Do not merge your own PR.* This ensures that at least one other person has reviewed the suggested changes and has approved them. The only exception is if another person specifically "approves" your PR, but doesn't actually merge it. In this scenario, because the other person gave approval that they agree with the changes, the original PRer is allowed to merge their own changes.
- *Be clear and descriptive in PR comments.* Include references to current GitHub issues when appropriate. Reference specific people if the PR addresses someones concerns. Include reasoning behind changes that could be controversial (e.g. reason why a field name changes, but no reason needed to fix a typo in a field description).

