# Lab 2 - UrlCount Solution

## Overview
This lab modifies the provided Hadoop WordCount application into a UrlCount application using Java and Hadoop MapReduce. 
The goal is to extract URLs from the two Wikipedia input pages, count their occurrences, and report only URLs appearing more than 5 times.

## Implementation

- Used the **Java Hadoop implementation** and created `UrlCount.java` from the provided `WordCount1.java`.
- Modified the Mapper using Java `Pattern` and `Matcher` to extract URLs from `href="..."` attributes.
- Retained the Reducer logic to output only URLs with a total count **greater than 5**.
- Updated the Makefile with targets to compile `UrlCount.java` into `UrlCount.jar` and run the UrlCount application.

## Software / Environment

- Java
- Hadoop 3.3.6
- Google Cloud Dataproc
- Git/GitHub
- CSEL Coding environment

The program was first developed and tested in the CSEL environment and then executed on Google Cloud Dataproc.

## Dataproc Results

| Configuration | Execution Time |
|---|---|
| 1 master + 2 workers | 1m 6.778s |
| 1 master + 4 workers | 1m 20.760s |

Execution time was measured using the `time` command.

### 2-Worker Output

```text
#                                               18
https://en.wikipedia.org/wiki/Google_File_System 6
https://en.wikipedia.org/wiki/ISBN_(identifier)  18
https://en.wikipedia.org/wiki/S2CID_(identifier) 14
mw-data:TemplateStyles:r1295599781                33
mw-data:TemplateStyles:r886049734                 12
https://en.wikipedia.org/wiki/Doi_(identifier)   18
https://en.wikipedia.org/wiki/MapReduce           6
mw-data:TemplateStyles:r1333133064                 7
mw-data:TemplateStyles:r1333433106               121
```

### 4-Worker Output

```text
https://en.wikipedia.org/wiki/Doi_(identifier)   18
mw-data:TemplateStyles:r886049734                 12
https://en.wikipedia.org/wiki/ISBN_(identifier)  18
mw-data:TemplateStyles:r1295599781                33
#                                               18
https://en.wikipedia.org/wiki/S2CID_(identifier) 14
mw-data:TemplateStyles:r1333133064                 7
https://en.wikipedia.org/wiki/Google_File_System  6
https://en.wikipedia.org/wiki/MapReduce            6
mw-data:TemplateStyles:r1333433106               121
```

Both configurations produced the same URL counts, although the ordering of the output differed. All reported URLs have a count greater than 5, as required.

## Execution Time Discussion
- The 2-worker cluster completed in **1m 6.778s**, while the 4-worker cluster took **1m 20.760s**. It was interesting that the 4-worker configuration was slower even though it had more processing resources.
- Since the input consists of only two Wikipedia pages, the workload is relatively small. The additional scheduling, communication, and coordination overhead of using more workers can outweigh the benefits of additional parallelism for a small dataset.

## Dataproc Configuration
- Both experiments used `e2-standard-2` master and worker machines in `us-east4`. The first cluster used 2 workers and the second used 4 workers.
- The default Dataproc disk allocation caused an insufficient `DISKS_TOTAL_GB` quota error, so `--master-boot-disk-size=100GB` and `--worker-boot-disk-size=100GB` were used to create the clusters within the available quota.

## Resources and Collaboration
- Used the Hadoop tutorial and other resources linked in the assignment README.
- Used ChatGPT Edu to help understand the provided starter code, README, Hadoop/MapReduce concepts, and the Dataproc workflow.
- After understanding the requirements and existing code, I worked through the implementation changes and testing for the solution.
- Any collaboration with other people was done according to the course collaboration policy.

## Conclusion
- The UrlCount application successfully extracts URL references from the Wikipedia input files, counts their occurrences using Hadoop MapReduce, and reports only URLs occurring more than five times.
- The application was successfully tested on both 2-worker and 4-worker Dataproc clusters. For this small dataset, the 2-worker configuration completed faster than the 4-worker configuration.