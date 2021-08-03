---
title: "Automation and Monitoring"
chapter: true
weight: 30
pre: "<b>4. </b>"
---

# Automation and Monitoring

### Automate Actions and Monitoring your Scans

After Cloud One - File Storage Security completes a scan, the scan results are tagged to the file and published to Amazon SNS topic ```ScanResultTopic```.

If you want to do more with the results, you'll have to create or add a post-action to take place after the scan. We provide many samples of integrations that can be done with Cloud One File Storage Security in our GitHub page, one of the most used Post-Actions is the ability to send clean files to one Amazon S3 bucket (promote) and send malicious files to another Amazon S3 bucket (quarantine). We also provide an API to help with creating your own post-scan actions  :laptop:

### Contributing

If you encounter a bug, think of a useful feature, or find something confusing in the docs, please
[create a new issue](https://github.com/trendmicro/cloudone-filestorage-plugins/issues/new)!

We :sparkling_heart: pull requests :laptop: :right_arrow: :cloud:. If you'd like to fix a bug, contribute to a feature or
just correct a typo, please feel free to do so.

If you're thinking of adding a new feature, consider opening an issue first to discuss it to ensure it aligns to the direction of the project (and potentially save yourself some time!).


#### Contribution Guidelines:

- Fork this repository.
- Commit your code with a message that is structured according to the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.2/) specification.
- Submit a PR and request a review:  [Submit PR](https://github.com/trendmicro/cloudone-filestorage-plugins/pulls/new).

--------

Let's start creating the post-actions for automation and monitoring :rocket: