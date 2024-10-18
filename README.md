# pulsar-sql module extracted from apache/pulsar

### History

Pulsar SQL has been removed from the apache/pulsar repository. Some history:

* In April 2020, it was decided to remove Pulsar SQL from apache/pulsar repository:
  * https://lists.apache.org/thread/qr49rp91w8pp8ljz1jtxn92ktnznh9tb
  * https://github.com/apache/pulsar/wiki/PIP-62%3A-Move-connectors%2C-adapters-and-Pulsar-Presto-to-separate-repositories
* In April 2021, there was a decision to contribute Pulsar SQL as a Trino plugin:
  * https://lists.apache.org/thread/f9n6sc2mrboq5sxhjbr7gvdl8vqp9fpk
* This was the status in December 2021:
  * https://lists.apache.org/thread/gqs9tq3p6tr38b7j6gjbfzfg7z1vkvsd
  * The Trino PR was https://github.com/trinodb/trino/pull/8020
  * The work on the plugin stalled and it was never taken to completion.
* In December 2023, Pulsar SQL was finally removed from the apache/pulsar repository:
  * https://lists.apache.org/thread/4f1cco12cycq36m7vtyjs2j5q5975666
* In October 2024, the Pulsar SQL modules were extracted from apache/pulsar to this repository, apache/pulsar-sql.

### Extracting the Pulsar SQL modules with preserved commit history

This repository is a fork of the Pulsar SQL module files that were present in apache/pulsar until [the removal commit](https://github.com/apache/pulsar/commit/c157f0d9). This commit is used to find out the list of removed files. Since the Pulsar SQL modules have been maintained in branch-3.0, the files have been extracted from branch-3.0 on 2024-10-18 to this repository.

It has been extracted from apache/pulsar with the following commands so that the commit history is preserved:

```shell
git clone https://github.com/apache/pulsar pulsar-sql
cd pulsar-sql
# https://github.com/apache/pulsar/commit/c157f0d9 is the commit hash of Pulsar SQL modules removal
git diff-tree --no-commit-id --name-only --diff-filter=D -r c157f0d > /tmp/keep_files.txt
# branch-3.0 contains the maintained Pulsar SQL modules
git checkout branch-3.0
# also keep all files in the pulsar-sql directory in branch-3.0
find pulsar-sql >> /tmp/keep_files.txt
# install git-filter-repo for filtering out the removed Pulsar SQL modules
brew install git-filter-repo
# filter out the removed Pulsar SQL modules
git filter-repo --paths-from-file /tmp/keep_files.txt --force
# delete the old master branch
git branch -D master
# create a new master branch
git checkout -b master
```

### Todo

- [ ] Setup a maven build for the separate repository
  - Reuse apache/pulsar and outdated [apache/pulsar-presto](https://github.com/apache/pulsar-presto) pom.xml and setup parent pom.xml for the separate repository
- [ ] Setup a GitHub Actions CI pipeline to test the separate repository
