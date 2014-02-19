# Special Collections Finding Aids EAD Repository

This repository contains a snapshot of the Archivists' Toolkit-exported EADs describing finding aids from various NYU-associated special collections. This data is harvested into a Solr index for the [finding aids discovery portal](https://github.com/NYULibraries/findingaids).

When the discovery portal is in production, this repos will contain a live cut of the EAD data and the publishing tool will have a hook to push out changes, therefore maintaining one up-to-date repository of EADs.

## Polling Changes and Reindexing

A Jenkins job is triggered every time this index is updated. This job calls [a reindex task](https://github.com/NYULibraries/findingaids/blob/master/lib/tasks/findingaids.rake#L61) which gets the previous commit and updates the Solr index with all the changed files.

There are also [institution-level automated jobs](http://jenkins.library.nyu.edu:8080/view/Finding%20Aids/) for running a full reindex in case things get out of sync. Note that these full reindex jobs can take up to 24 hours.

[![Build Status](http://jenkins.library.nyu.edu/buildStatus/icon?job=Finding Aids Development Solr Reindex Changed)](http://jenkins.library.nyu.edu/job/Finding%20Aids%20Development%20Solr%20Reindex%20Changed%20Cron/)

**Technical Note:** This job actually calls a downstream job with the full finding aids rails environment and updates this EAD repository as a subfolder before running the reindex task.

This EAD repository is not included as a submodule in the finding aids project because I don't want the Jenkins trigger for that task to rebuild everytime this repos is updated. Since the data is in a Solr index, keeping these repositories completely separate is a fine solution. However, when polling changes with git I want to have the full rails app environment handy to use the built-in solr_ead index updater.

## Reporting Issues

The special collections content owners can [create issues here](https://github.com/NYULibraries/findingaids_eads/issues) if they are relevant to data errors in the EADs.