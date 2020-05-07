# Special Collections Finding Aids EAD Repository

This repository contains a live cut of the Archivists' Toolkit-exported EADs describing finding aids from various NYU-associated special collections. This data is harvested into a Solr index for the [finding aids discovery portal](https://github.com/NYULibraries/findingaids).

The [EAD publishing tool](https://github.com/NYULibraries/git_transactor) has a hook to push out changes here, therefore maintaining one up-to-date repository of EADs.

## Polling Changes and Reindexing

### Reindexing Changed Files In Real-time [![Build Status](http://jenkins.library.nyu.edu:8080/buildStatus/icon?style=flat&job=Finding Aids Production Solr Reindex Changed)](http://jenkins.library.nyu.edu:8080/view/Finding%20Aids/job/Finding%20Aids%20Production%20Solr%20Reindex%20Changed/)

A Jenkins job is triggered every time this index is updated. This job calls [a reindex task](https://github.com/NYULibraries/findingaids/blob/master/lib/tasks/findingaids.rake#L61) which gets the previous commit and updates the Solr index with all the changed files.

### Reindexing Changed Files Nightly [![Build Status](http://jenkins.library.nyu.edu:8080/buildStatus/icon?job=Finding Aids Production Nightly Solr Reindex Changed Cron&style=flat)](http://jenkins.library.nyu.edu:8080/view/Finding%20Aids/job/Finding%20Aids%20Production%20Nightly%20Solr%20Reindex%20Changed%20Cron/)

In addition to real-time updates we run a nightly job that reindexes any files changes in commits over the past 24 hours. This serves as a failsafe for any failed Jenkins rebuilds or Jenkins downtime.

### Full Reindex [![Build Status](http://jenkins.library.nyu.edu:8080/buildStatus/icon?style=flat&job=Finding Aids Production Solr Full Reindex Cron)](http://jenkins.library.nyu.edu:8080/job/Finding%20Aids%20Production%20Solr%20Full%20Reindex%20Cron/)

There is also a [automated job](http://jenkins.library.nyu.edu:8080/view/Finding%20Aids/) for running a full reindex in case things get out of sync. **Note that these full reindex jobs can take up to 5 days.**

**Technical Note:** This job actually calls a downstream job with the full finding aids rails environment and updates this EAD repository as a subfolder before running the reindex task.

This EAD repository is not included as a submodule in the finding aids project because I don't want the Jenkins trigger for that task to rebuild every time this repos is updated. Since the data is in a Solr index, keeping these repositories completely separate is a fine solution. However, when polling changes with git I want to have the full rails app environment handy to use the built-in solr_ead index updater.

## Known problematic commit range

The commits within range [5a67a80](https://github.com/NYULibraries/findingaids_eads/commit/5a67a801e81563e1d88768357bd520bcddecee40) to [bfb2f0e](https://github.com/NYULibraries/findingaids_eads/commit/bfb2f0e848d66b464bfcb418da86519a20b167c0) (inclusive) are known to corrupt the state of the repo on case-insensitive filesystems.  Note that Macos and Windows filesystems default to case-insensitive.  For full details on this issue, see Jira ticket [DLFA-155: Duplicate finding aids and filename collision in findingaids_eads Github repo](https://jira.nyu.edu/jira/browse/DLFA-155).

## Reporting Issues

The special collections content owners can [create issues here](https://github.com/NYULibraries/findingaids_eads/issues) if they are relevant to data errors in the EADs.
