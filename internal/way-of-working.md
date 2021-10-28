
# Documentation way of working

The goal of the Monite documentation is to enable all Monite
customers to easily integrate financial engagement logic into their apps.

Monite writing is based on the following pillars:

-   **Technical correctness** - All information about Monite products
    must explain clearly the purpose of the product and how to easily
    implement the main business use-cases.

-   **Consistency** - Monite documentation must be written the same
    way for all projects. All documentation is written using our
    [templates](templates/README.md). The templates are based on
    [Darwin information typing](https://en.wikipedia.org/wiki/Darwin_Information_Typing_Architecture#Information_typing)
    and implemented in GitHub Markdown. The vast majority of our documentation is task based to give customers a sense of continuous achievement.

-   **Linguistic correctness** - For language, we follow the [Google
    developer documentation style guide](https://developers.google.com/style/highlights) with a couple of Monite specializations. 

# Write accurate documentation efficiently

For everyone to actively participate in this goal, best practice is to
adopt a [docs-as-code](https://docs-as-co.de/) methodology. That is, you
write, review and validate documentation with the same tools that you
use to write code.

GitHub Markdown is a human-readable document format, it is compatible with both stoplight and GitHub which makes it ideal. 

The tools we use for our docs-as-code approach are:

- Issue Trackers -
    [Notion](https://www.notion.so/monite/)

- Version Control -
    [GitHub](https://github.com/billy-the-fish/stoplight-play)

- Plain text markup -
    [Github Markdown](https://guides.github.com/features/mastering-markdown/)

- Writing tools - either:
  - Add the Markdown and OpenAPI plugins to your developer IDE.
  - Use the [Stoplight IDE](<link to final URL>)

- Documentation review - [GitHub Pull
    Request](https://docs.gitlab.com/ee/user/project/merge_requests/)

- Automated Tests - link and output validity. The tools to test during
    this POC are listed in [Publication](#publication).

# The documentation process

The process to document a new feature or update existing documentation
is that the:

1.  Product Manager/Owner or their proxy creates a ticket for feature to
    be documented that matches the [Definition of
    ready](#definition-of-ready).

2.  Assigned writer creates a branch with the same name as the ticket
    and works on the documentation in their IDE of choice using the:

    -   [Templates](templates/README.md)

    -   [Google developer documentation style
        guide](https://developers.google.com/style/highlights)

    -   [Documentation folder
        structure](#documentation-folder-structure)

3.  Writer creates a [pull
    request](https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/merging-a-pull-request) and sets at least 1 Subject Matter Expert as the reviewer.

4.  Writer moves the ticket to *In Review* and assigns it to the SME.

5.  Once any updates have been incorporated, the SME approves the merge
    request and assigns the issue back to the writer.

6.  If the PR has a lot of updated text, the writer assigns the ticket
    to a colleague for language review and assigns the ticket to them.

7.  Once any updates have been incorporated, the language reviewer
    approves the merge request and assigns the issue back to the
    writer.

8.  Writer merges the PR and moves the ticket to *Done*.

## Collaboration of developers and technical writers
The workflow for developer and technical writers to collaborate is on the scheme below (in BPMP notation):
![](../images/API_Documenting_Workflow.png)
In short, each party (a dev, a TW) works on their own task. When a new version of swagger spec is ready (e.g. due to a new end-point is added), then the developer publishes it [here](https://api.dev.monite.dev/partner-api/docs) and notifies the writer about the update via Slack or other messenger.

When notified, the writer commits his work into the repository. Then he downloads updated openapi.json, puts/replaces it in the corresponding branch and fulfill merging. When successfully completed, the updated version is published in Stoplight.

## Branching and merging strategy
Decision to use git to support API documenting process makes this process much more flexible: several git branches can be used to independently modify the swagger specification (openapi.json) by different teams (developers, tech. writers). The key issue to keep consistency though is to have a reliable merging strategy, i.e. to have answers on the following questions:
- how many branches do we need
- when to create each of these branches; who does that
- what is the merging strategy? (three way, recursive 3 way, rebase, or something else)
- whether and when to delete branches
- who and how administers the repo?
Below is the scheme of branching and merging strategy to use.
![](../images/Branching_merging_strategy.png)
3 branches are used:
- master which contains the final version to publish. It is never deleted.
- Technial Writers's branch which temporary contains writers's amendments. It is deleted each time a new revision of openapi.json is ready. After merging the file into the master branch, TW's branch is reacreated.
- Developer's branch created to store all revision of openapi.json produced by the developer team. It is never deleted.

# Definition of ready

To work efficiently, each ticket should have:

-   **Business case** - why we invest time into building/documenting
    this feature, when applicable.

-   **Fix version** -when applicable

-   **Subject Matter Expert** - to clarify details

-   **Reviewer** - to validate the final version, often the
    reporter/creator.

-   Linked to an **Epic** - when applicable

-   **Scope** - the files and specs that will used to write the docs

-   **Estimated** - granular enough ⇒ Consider the over head if it is
    less then 2 hours, consider making it smaller if it is larger then 2
    days.

-   **Acceptance criteria** - a list of features, business use cases,
    descriptions of what should be done

-   **Additional valuable information** - design documentation, meeting
    notes, code examples. UX documentation, and so on.

If this information is already included in another ticket, this ticket
can refer back without rewriting everything.

When issues do not have this information, it should be made clear that
any estimates on duration and so on will be unreliable. Starting without
this information is discouraged, and the considerations should be made
clear to those involved.

# Definition of done

For a feature to be correctly documented, the documentation:

-   Explains why you should use a feature, how the feature works, how to
    use it. Reference material must be complete.

-   Has passed technical review. This is the same as a code review.

-   Adheres to page structure, language and style guides.

-   Where time is allocated, reviewed by somebody outside the team that
    made the feature or by a {ADR}.

# Documentation folder structure

Monite documentation uses the [stoplight project structure](https://meta.stoplight.io/docs/studio/ZG9jOjY0-working-with-projects#project-structure). This separates reference from procedural docs. The final structure is to be decided. For example, [see how stoplight write their own docs](https://github.com/stoplightio/platform-docs).

# Publication

Documentation is published in the following formats:

-   GitHub - the Monite Github is open source for anyone to read or update using a pull request. 

-   Stoplight - the [Stoplight platform](https://meta.stoplight.io/docs/platform/ZG9jOjIwNjk2MQ-welcome-to-the-stoplight-docs) supplies all the features for us to write and publish using a docs-as-code methodology. 

Testing tools to investigate:

-   <https://github.com/errata-ai/vale> - a command-line tool that
    brings code-like linting to prose. Can implement the Google Style
    guide. <https://github.com/errata-ai/styles>
