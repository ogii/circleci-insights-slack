# circleci-insights-slack
A example showing how to send CircleCI Insights data to Slack

## Table of Contents

- [Setup](#setup)
- [Usage](#usage)
- [Support](#support)

## Setup

Fork the project, and set it up on CircleCI. Following that, please create a [CircleCI Personal API Token](https://app.circleci.com/settings/user/tokens) and save it as a project level or contenxt variable named `CIRCLE_TOKEN`.

Additionally, you will need to create and store the following variables as project variables or in a context:
1. `SLACK_ACCESS_TOKEN`
2. `SLACK_DEFAULT_CHANNEL`

Lastly, you will need to specify the following parameters for the `insightsdata` job:
1. `project-name` - the name of your project
2. `org-name` - the name of your organization
3. `vcs-name` - the name of your vcs (either `gh` or `bb`)
4. `workflow-names` - a comma delimited string of the workflow names you wish to see data for.
5. `reporting-window` - the time period you want to get data for. Possible values are: `"24-hours"`, `"7-days"`, `"30-days"`, `"60-days"`, and `"90-days"`.

You can additional set up a scheduled pipeline to run the workflow periodically. Assuming you create a scheduled pipeline with the name ``, a sample workflow definition would look like the following:

```
workflows:
  insightdata:
    when:
      and:
        - equal: [ scheduled_pipeline, << pipeline.trigger_source >> ]
        - equal: [ "insight_schedule", << pipeline.schedule.name >> ]
    jobs:
      - insightsdata:
          project-name: "insights-test-repo"
          org-name: "testorg"
          vcs-name: "gh"
          workflow-names: "build_all_images,daily"
          reporting-window: "30-days"
          context:
            - slack-secrets
```

**note: This will get the data for all branches in the specified project.**

## Usage

You can either manually trigger the `insightdata` workflow via the API or the `Trigger Pipeline` button in the UI.

Additionally, you can set up a [Scheduled Pipeline](https://circleci.com/docs/scheduled-pipelines) to trigger the workflow at a designated interval.

## Support

Please [open an issue](https://github.com/ogii/circleci-insights-slack/issues/new) for support.
