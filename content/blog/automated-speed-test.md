---
title: "Automated Speed Test"
date: 2016-07-07T00:00:00-05:00
draft: false
tags: ["projects", "automation", "cron", "aws"]
description: "A brief guide to setting up an automated internet speed test at home."
meta_img: "/img/favicon.png"
hacker_news_id: ""
---

Set up a new [DynamoDB](https://aws.amazon.com/dynamodb/) table called `speedtest_results` with `Timestamp` as the primary sort key

{{< figure src="https://s3.amazonaws.com/dpfrakes/dynamodb.png" >}}

Install [`speedtest-cli`](https://github.com/sivel/speedtest-cli) via `pip`

```bash
pip install speedtest-cli
```

Install `awscli` via `pip` and configure your [AWS credentials](https://aws.amazon.com/iam/)

```bash
# install tools
pip install awscli boto3

# authenticate
aws configure
```

Script to run a speed test and upload the results to AWS

{{< gist dpfrakes eabc018acff09b8384f4e8ca0dfb5602 >}}

The [cron job](https://en.wikipedia.org/wiki/Cron) that runs the script every 15 minutes

```bash
# crontab -e
*/15 * * * * python /Users/dpfrakes/dev/speedtest/run_speedtest.py
```
