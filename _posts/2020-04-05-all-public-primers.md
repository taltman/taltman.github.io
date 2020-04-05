---
layout: post
title: "Design Flaws in COVID-19 Primers from Multiple International Labs"
---

## Summary

Not only do the CDC COVID-19 primer-probe sets have design flaws, but
other publicly-available primer-probe sets from government labs around
the globe have their own flaws as well.

## Methodology

I sought to perform detailed primer design analysis on all available
COVID-19 primer-probe sets to allow for a fair comparison of whether
such flaws are wide-spread, or particular to the CDC's designs. I
collated a set of primer-probe sets as found in [a pre-print by Jung
et al.](https://www.biorxiv.org/content/10.1101/2020.02.25.964775v1)
and [Corman et
al.](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6988269/)
into a FASTA file available
[here](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/kit-primers/all-COVID-19-kit-primers.fasta).
I applied the same analysis approach as described [in my original post
regarding COVID-19 design
flaws](https://tomeraltman.net/2020/03/03/technical-problems-COVID-primers.html)
unless otherwise noted. One complication I encountered with the
expanded set of primers was the presence of ambiguous bases in some
oligos, sometimes more than one per oligo. This is an issue as Primer3
cannot analyze such sequences. This was handled by expanding such
sequences into all implicit combinations of unambiguous bases, and
using Primer3 to analyze the sequences. In the results table, you will
note the ID sub-field of "subset": "subset-0" means that the original
sequence was used as-is; "subset-N" for N=1,2,3,... represented
enumerated sequences from the original sequence with ambiguous bases.

## Results

The [comparison table of all primer-probe
sets](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/reports/all-public-primers-qc-report.csv)
versus a full set of tests for the forward primer, reverse primer, the
hybridization probe, and combinations thereof is available on the
[COVID-19 Primer Data BitBucket
repository](https://tomeraltman.net/2020/03/11/COVID-19-candidate-primers-update.html).
In the CSV file, values surrounded by "**" indicate a value that is
above or below a quality control threshold (i.e., a design flaw).

In the next sections I will describe design flaws detected, organized
by the origin of the primer-probe sets (in no particular order):

### CDC

In my initial analysis of the CDC primers, I restricted myself to just
the primers targeting the SARS-CoV-19 genome. In this latest analysis,
I included the control primer-probe set targeting the RdRP gene:

* The forward primer has five out of five of the 3' bases as G/C (GC clamp)

### China

* China-N: A high-temperature hairpin loop in the reverse primer
* China-N: A high-temperature hairpin loop in the hybridization probe
* China-N: The hybridization probe's Tm is not at least 6 degrees Celsius higher than the primers' Tm
* ORF1ab: The hybridization probe has a high-temperature hairpin loop

### EU-Drosten

* EU-Drosten-N's reverse primer's 3' end GC clamp has four G/C bases in the last five base positions
* EU-Drosten-N's hybridization probe has a high-temperature hairpin loop
* EU-Drosten-N's probe Tm is not at least 6 degrees Celsius higher than the primers' Tm
* EU-Drosten-RdRP-P1 and EU-Drosten-RdRP-P2: forward primer has five out of five 3' bases as G/C (GC clamp)
* EU-Drosten-RdRP-P1 probe: 20 out of 32 sequences have high-temperature hairpin loops
* EU-Drosten-RdRP-P1: 28 out of 32 explicit primer-probe sets have less than 6 degrees Celsius difference between probe Tm and primer Tm
* EU-Drosten-RdRP-P2 probe: has a very high hairpin loop Tm of 79.14 degrees Celsius
* EU-Drosten-RdRP-P2: explicit primer-probe sets have less than 6 degrees Celsius difference between probe Tm and primer Tm

### Hong Kong

* HKU-ORF-1b forward primer: 8 out of 16 explicit sequences have high hairpin loop Tm
* HKU-N forward primer: high hairpin loop Tm
* HKU-ORF-1b reverse primer: four out of five 3' bases are G/C (GC clamp)
* HKU-N probe: First 5' base is G (interferes with fluorescence)
* HKU-ORF-1b probe: 8 out of 16 explicit sequences have high temperature hairpin loops
* HKU-N probe: high temperature hairpin loop
* HKU-ORF-1b & HKU-N: all explicitly primer-probe sets have less than 6 degrees Celsius difference between probe Tm and primer Tm

### Japan

* Forward primer has high-temperature hairpin loop


### Thailand

* The probe Tm is not 6 degrees Celsius higher than the primers' Tm

## Discussion

A thorough check for common design flaws among an international set of
primer-probe sets shows that many other countries have similar issues
as the CDC primer-probe sets. While there is no guarantee that a
theoretically-optimal primer-probe set will perform well /in vitro/
(and, conversely, there are many "flawed" primer probe sets in various
applications that have been observed to work well in practice), it is
inefficient to ignore these common pitfalls. This is because it only
takes seconds for tools like Primer3 to screen these primer-probe sets
for common flaws, while it takes significantly more time and resources
to perform similar tests at the wet bench. Especially bearing in mind
that these government labs are racing against time to develop
efficient and accurate primer-probe sets at scale for testing entire
populations against a rapidly-spreading pandemic, there should be no
tolerance for testing theoretically-flawed primer-probe designs, when
there are others free of such flaws available for experimental
validation. While not necessarily optimal (and definitely not
validated /in vitro/), I was able to generate such [primer-probe sets
targeting
SARS-CoV-19](https://tomeraltman.net/2020/03/11/COVID-19-candidate-primers-update.html)
without these design flaws. Furthermore, all of the oligos in the
primer-probe sets that I generated are unambiguous; better not to
dilute one's oligos needlessly with degenerate primer design. It is my
hope that going forward there can be standardization around the primer
design process, to allow these design flaws to be avoided
systematically by government labs worldwide.


