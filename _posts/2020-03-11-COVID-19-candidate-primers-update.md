---
layout: post
title: "Updated set of COVID-19 Candidate Primers"
---

In the wake of [my previous blog
post](https://tomeraltman.net/2020/03/03/technical-problems-COVID-primers.html),
I have received numerous bits of advice on how to make my set of
COVID-19 candidate primers better. Today I am releasing my second
version of the candidate primers. Here is a summary of the updates:

* All data resources are being updated on a [Git repository on
  BitBucket](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/),
  allowing easy sharing among bioinformaticists. Feel free to fork &
  send me pull requests to make this resources better.

* [Latest spreadsheet report (version 2)](https://bitbucket.org/tomeraltman/covid-19-primer-data/downloads/new-COVID-primer-set-stats_v2.xlsx) with recommended candidate primers
  highlighted in green (marginal primers highlighted in yellow).

* Added many additional columns to the report to help in candidate
  primer probe set selection, including %GC of the oligos, the melting
  temperature of the oligos, and amplicon %GC and length (relative to
  the reference RefSeq COVID-19 complete genome). Column 'U' in
  particular reports the difference between the probe melting
  temperature, and the highest melting temperature of the forward and
  reverse primers. You want this difference to be larger than 6
  degrees Celsius, to allow for efficient probe hybridization. I sort
  the report first on the F1-score (column 'M', descending) and the
  probe-primers melting temperature difference (column 'U',
  descending), to make it easy to find the best primer-probe sets.

* Documentation on [the meaning of the
  columns](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/doc/column-descriptions.md)
  in the spreadsheet report.

* A [FASTA file of COVID-19 primer-probe sets](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/kit-primers/all-COVID-19-kit-primers.fasta) that are currenlty
  deployed in testing kits.

* Fixing of the NCBI Blast database sequence identifier issue (see
  original post)

* Improvements to the amplicon-calling code and other fixes to improve
  performance


I hope this is another step towards making this work more useful and
accessible. Please let me know if you have any technical questions or
bug reports (code bugs or documentation bugs) by creating
an issue in the [Issue
Tracker](https://bitbucket.org/tomeraltman/covid-19-primer-data/issues?status=new&status=open). For
all else, you can find me on Twitter or email me (see bottom of page).
