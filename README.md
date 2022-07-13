# circleci-insights-slack
A example showing how to send CircleCI Insights data to Slack

## Table of Contents

- [Setup](#setup)
- [Usage](#usage)
- [Support](#support)

## Setup

Fork the project, and set it up on CircleCI. Following that, please create a [CircleCI Personal API Token](https://app.circleci.com/settings/user/tokens) and save it as a project variable named `CIRCLE_TOKEN`.

Additionally, you will need to create and store the following variables as project variables or in a context:
1. `SLACK_ACCESS_TOKEN`
2. `SLACK_DEFAULT_CHANNEL`

Lastly, you will need to specify the following parameters for the `insightsdata` job:
1. `projectslug` - the slug of your project. It takes the following form: `"gh|bb/orgname/projectname"`
2. `workflow-names` - a comma delimited string of the workflow names you wish to see data for.
3. `reporting-window` - the time period you want to get data for. Possible values are: `"24-hours"`, `"7-days"`, `"30-days"`, `"60-days"`, and `"90-days"`.

Example workflow configuration:

```
workflows:
  insightdata:
    jobs:
      - insightsdata:
          projectname: "insights-test-repo"
          org: "testorg"
          workflow-names: "build_all_images,daily"
          reporting-window: "7-days"
          context:
            - slack-secrets
```

**note: This will get the data for all branches in the specified project.**

## Usage

You can either manually trigger the `insightdata` workflow via the API or the `Trigger Pipeline` button in the UI.

Additionally, you can set up a [Scheduled Pipeline](https://circleci.com/docs/scheduled-pipelines) to trigger the workflow at a designated interval.

## Support

Please [open an issue](https://github.com/ogii/circleci-insights-slack/issues/new) for support.
