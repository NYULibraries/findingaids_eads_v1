# Special Collections Finding Aids EAD Repository

This repository contains a live cut of the ArchiveSpace-exported EADs describing finding aids from various NYU-associated special collections. This data is harvested into a Solr index for the [finding aids discovery portal](https://github.com/NYULibraries/specialcollections).

The [EAD publishing tool](https://github.com/NYULibraries/git_transactor) has a hook to push out changes here, therefore maintaining one up-to-date repository of EADs.

## Polling Changes and Reindexing

**[The full documentation for the K8s jobs is in a private repos now](https://github.com/NYULibraries/nyulibraries_kubernetes/tree/master/charts/specialcollections)**

### Reindexing changed files in real-time

A K8s job is triggered every time this index is updated. This job calls [a reindex task](https://github.com/NYULibraries/findingaids/blob/master/lib/tasks/findingaids.rake#L61) which gets the previous commit and updates the Solr index with all the changed files.

### Reindexing changed files nightly and weekly 

In addition to real-time updates we run a nightly job that reindexes any files changes in commits over the past 24 hours and a weekly job that reindexes changes in commits over the past 7 days. This serves as a failsafe for any failed rebuilds.

### Full reindex

There is also a job for running a full reindex in case things get out of sync. **Note that these full reindex jobs can take up to 5 days.**

### Single EAD reindex

We can also run single index jobs for a named EAD.

## Known problematic commit range

The commits within range [5a67a80](https://github.com/NYULibraries/findingaids_eads/commit/5a67a801e81563e1d88768357bd520bcddecee40) to [bfb2f0e](https://github.com/NYULibraries/findingaids_eads/commit/bfb2f0e848d66b464bfcb418da86519a20b167c0) (inclusive) are known to corrupt the state of the repo on case-insensitive filesystems.  Note that Macos and Windows filesystems default to case-insensitive.  For full details on this issue, see Jira ticket [DLFA-155: Duplicate finding aids and filename collision in findingaids_eads Github repo](https://jira.nyu.edu/jira/browse/DLFA-155).

## Reporting Issues

The special collections content owners can [create issues here](https://github.com/NYULibraries/findingaids_eads/issues) if they are relevant to data errors in the EADs.
