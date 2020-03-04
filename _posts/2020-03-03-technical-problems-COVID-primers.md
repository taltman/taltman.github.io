---
layout: post
title: "Technical Problems with Existing COVID-19 Primers, and an
Improved Set of Primers"
---

[My apologies in advance for the draft quality of this post, as it was written in haste. -TA]

# Summary

I review technical problems with the CDC COVID-19 primers and I describe how I 
generated a new set of primers and probes without those problems. The
ten best primer-probe pairs have perfect classification performance
(recall, precision, and F1-score all 1.0).

This blog post strives to describe a problem with some of the COVID-19
primer-probe sets that are being promoted for the development of
diagnostics. I describe the issues in terms of the primer design
process, and with regards to validating the performance of the
existing primers using 'e-PCR'.

I then go on to develop a bioinformatic pipeline to design better
candidate primers that pass stringent design criteria, and have a better
demonstrated performance than the existing COVID-19 primers from the
CDC. I provide the candidate primers for free, under a Creative
Commons license (download
[here](/assets/new-COVID-primer-set-stats.csv)). Lastly I identify
next steps in this project.

My hope is that this will help start a conversation about the design
process for these critical diagnostic primers. Perhaps starting with
these primers, and with input from other scientists, we can develop a
set of candidate primers for COVID-19 that could be used by health
agencies and private diagnostic kit manufacturers alike to not only
generate more diagnostic kits, but *better* ones.

# Primer Design Issues

As the COVID-19 pandemic spread from country-to-country over the past
month, I felt that I should try to help in some way. I had recently
been working on primer design as part of my consultancy, and I thought
that perhaps I can analyze the primers developed by the CDC and
others, in part to see how their primers stack up against the ones
that I had developed for a different project.

As others have noted, there are flaws in the [COVID-19 diagnostic
primers from the
CDC](https://www.cdc.gov/coronavirus/2019-ncov/lab/index.html). They
released four primer-probe sets; three that target the N gene, which
encodes for a nucleocapsid phosphoprotein, and one that targets a
human RNA polymerase gene (as part of laboratory controls).

Using [Primer3](https://primer3.org), a respected software program for
predicting performant primer probe sets using thermodynamic
calculations, I analyzed the three N gene primer probe sets. I found
the following problems with each set:

## N1

Here is primer probe set N1, shown in Boulder format (as used by
Primer3):

    SEQUENCE_PRIMER=GACCCCAAAATCAGCGAAAT
    SEQUENCE_INTERNAL_OLIGO=ACCCCGCATTACGTTTGGTGGACC
    SEQUENCE_PRIMER_REVCOMP=TCTGGTTACTGCCAGTTGAATCTG

Here is the formatted analysis summary from Primer3 for N1:

    OLIGO            start  len      tm     gc%  any_th  3'_th hairpin seq
    LEFT PRIMER      28286   20   58.32   45.00    0.00   0.00    0.00 GACCCCAAAATCAGCGAAAT
    RIGHT PRIMER     28357   24   62.50   45.83    2.43   0.00   52.19 TCTGGTTACTGCCAGTTGAATCTG
    INTERNAL OLIGO   28308   24   69.19   58.33   13.48   0.00   42.14 ACCCCGCATTACGTTTGGTGGACC
    SEQUENCE SIZE: 29902
    INCLUDED REGION SIZE: 29902
    
    PRODUCT SIZE: 72, PAIR ANY_TH COMPL: 0.00, PAIR 3'_TH COMPL: 0.00

The reverse primer was rejected by Primer3 due to a predicted hairpin
loop. These loop formations can cause problems during PCR, leading to
lower amplification efficiency. The Primer3 output for the hairpin is
as follows:


    SEQUENCE_ID=CDC-N1-COVID-19
    Reverse primer:
    Tm: 52.2&deg;C  dG: -1596 cal/mol  dH: -34200 cal/mol  dS: -105 cal/mol*K
            5' TCTGGTTA*
                ||||   |
    3' GTCTAAGTTGACCGTC*

While lower than the default melting temperature cutoff, Primer3 found
more problems with the primer probe set. Namely, it can form
oligo-dimers with the reverse primer and the hybridization probe (with
themselves), and the hybridization probe itself can form a hairpin
loop:

Reverse primer self-dimer:

    Tm: 2.4&deg;C  dG: -5495 cal/mol  dH: -45600 cal/mol  dS: -129 cal/mol*K
     5' TCTGGTTACTGCCAGTTGAATCTG 3'
               |||| ||||
    3' GTCTAAGTTGACCGTCATTGGTCT 5'

Hybridization probe self-dimer:

    Tm: 13.5&deg;C  dG: -4045 cal/mol  dH: -87400 cal/mol  dS: -269 cal/mol*K
    5' ACCCCGCATTACGTTTGGTGGACC 3'
          || |   ||||   | ||
    3' CCAGGTGGTTTGCATTACGCCCCA 5'

Hybridization probe 3' hairpin loop:

    Tm: 42.1&deg;C  dG: -274 cal/mol  dH: -16800 cal/mol  dS: -53 cal/mol*K
           5' ACCCCGCA*
                  ||  T
    3' CCAGGTGGTTTGCAT*


## N2

Here is set N2:

    SEQUENCE_PRIMER=TTACAAACATTGGCCGCAAA
    SEQUENCE_INTERNAL_OLIGO=ACAATTTGCCCCCAGCGCTTCAG
    SEQUENCE_PRIMER_REVCOMP=GCGCGACATTCCGAAGAA

Just from inspection, you can see that there is an issue with a poly-X
run of five 'C's in the hybridization probe. Poly-X runs of five bases
or longer are known to cause problems with non-specific priming
(possibly leading to false positive readings), and are normally avoided.

Here is the formatted analysis summary from Primer3 for N2:

    OLIGO            start  len      tm     gc%  any_th  3'_th hairpin seq
    LEFT PRIMER      29163   20   58.75   40.00    0.00   0.00   34.76 TTACAAACATTGGCCGCAAA
    RIGHT PRIMER     29229   18   60.10   55.56   12.76   0.00   36.75 GCGCGACATTCCGAAGAA
    INTERNAL OLIGO   29187   23   68.15   56.52    8.63   0.00   49.09 ACAATTTGCCCCCAGCGCTTCAG
    SEQUENCE SIZE: 29902
    INCLUDED REGION SIZE: 29902
    
    PRODUCT SIZE: 67, PAIR ANY_TH COMPL: 0.00, PAIR 3'_TH COMPL: 0.00

Primer3 found self-dimer and hairpin loop issues with all of the
oligos in the N2 primer-probe set, though at lower temeratures.

## N3

Here is set N3:

    SEQUENCE_PRIMER=GGGAGCCTTGAATACACCAAAA
    SEQUENCE_INTERNAL_OLIGO=ATCACATTGGCACCCGCAATCCTG
    SEQUENCE_PRIMER_REVCOMP=TGTAGCACGATTGCAGCATTG

It has a four 'A' run at the 3' end of the forward primer. It also had
a predicted hairpin loop:

    SEQUENCE_ID=CDC-N3-COVID-19
    Reverse primer:
    Tm: 53.8&deg;C  dG: -1210 cal/mol  dH: -23500 cal/mol  dS: -72 cal/mol*K
       5' TGTAGCACG*
              |||  |
    3' GTTACGACGTTA*

Here is the formatted analysis summary from Primer3 for N3:

    OLIGO            start  len      tm     gc%  any_th  3'_th hairpin seq
    LEFT PRIMER      29163   20   58.75   40.00    0.00   0.00   34.76 TTACAAACATTGGCCGCAAA
    RIGHT PRIMER     29229   18   60.10   55.56   12.76   0.00   36.75 GCGCGACATTCCGAAGAA
    INTERNAL OLIGO   29187   23   68.15   56.52    8.63   0.00   49.09 ACAATTTGCCCCCAGCGCTTCAG
    SEQUENCE SIZE: 29902
    INCLUDED REGION SIZE: 29902
    
    PRODUCT SIZE: 67, PAIR ANY_TH COMPL: 0.00, PAIR 3'_TH COMPL: 0.00


# New Candidate Primers

The set of candidate primers can be downloaded
[here](/assets/new-COVID-primer-set-stats.csv). These are released as
[Creative Commons Attribution 4.0 International 4.0 (CC BY
4.0)](https://creativecommons.org/licenses/by/4.0/). I recommend
focusing on the top ten primer-probe sets, as they have the greatest
measured accuracy.

Below find selected details about how the primers were designed and
validated using an 'e-PCR' approach.


## Primer Design Criteria

I developed a bioinformatic pipeline that generates primers, using the
following criteria:

* Poly-X runs longer than four bases are not allowed
* Check that hybridization probes do not start with G (avoid quenching)
* Check that the GC clamp on the 3' end has one or two G's or C's
* GC% range between 40 and 60, with an optimum at 60
* Primer size range between 18 to 27 bases
* Hybridization probe size from 75 to 200 bases
* Disallow any hairpins, self-dimers, or oligo-interactions at ANY temperature
* Target regions conserved perfectly among all COVID complete genomes
* Exclude regions that are identical to [common cold-causing,
  human-associated coronaviruses](https://www.cdc.gov/coronavirus/general-information.html)
* Primers designed to cover the viral RNA polymerase gene, as it is
 highly conserved (Tom Slezak, former head of biodefense program at
 LLNL, private communication)


## Primer Performance Validation

Even if you design your primers using recommended settings with
trusted software like Primer3, there's still the chance that you'll
have off-target binding of the oligos to other stretches of DNA in
your sample, or even off-target amplicons if a forward and reverse
primer bind close enough to one another in the correct
orientation. For that reason, I performed a sequence homology search
using NCBI Blast+ against the NT database (downloaded
2020-03-02). Complete genomes (whether prokaryotic or viral) where the
forward and reverse primers, and the hybridization probe, all had
gapless alignments with 90% or greater sequence identity, and in the
proper orientation, were counted as a hit. I used all complete viral
genomes in NT with NCBI Taxonomy Database identifier 2697049 (the
identifier for the COVID-19 seuqences) as the "gold standard" for
assessing the performance of these primer probe sets as diagnostics
for COVID-19 presence (i.e., these are the sequences that the
primers should hit without fail).

The generated primers show that there are five false negatives, but
this is actually a bug in my current pipeline. It's due to an obscure
detail about how NCBI represents identical genomes in its Blast
databases. Those genomes are all identified, just under a different
identifier. So the top ten primer-probe pairs actually have perfect
performance: precision, recall, and F1-score all 1.0.


# Next Steps

Thank you for reaching this far. My hope is that I can start up a
conversation among scientists about how to design and validate
candidate primer probes /in silico/, to allow for faster and more
efficient wet-bench validation of the candidate primers. I hope this
will lead to better, more accurate COVID-19 diagnostic kits being
designed and successfully deployed around the country.

Please feel free to follow up with me via email or Twitter. I look
forward to constructive feedback about how to improve this analysis. I
am working on releasing the source code for the bioinformatic
pipeline, and writing up the methodology in more detail.  Thanks again
for your time, and thanks in advance for any help that you might
provide.

TODOs:
* Release software pipeline code
* Complete secondary structure code blocks for low-temperature entries
* Fix false negative reporting bug due to NCBI Blast databases
* Prepare technical manuscript detailing the methodology