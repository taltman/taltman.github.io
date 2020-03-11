---
layout: post
title: "Technical Problems with Existing CDC COVID-19 Primers, and an
Improved Set of Primers"
---


## Summary

In this post I review technical problems with the CDC COVID-19 primers
and I describe how I generated a new set of primers and probes without
those problems. 

Note that this was based on available outbreak whole-genome
sequence data obtained from the [NCBI Blast NT
database](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download),
downloaded from NCBI on 2020-03-02. I will try to re-run this pipeline
every few days to update the set of candidate primers in light of
newly-available sequence data. Please check these primers yourself
before using them for the basis of any diagnostic kit.

This blog post describes technical problems with some of the COVID-19
primer-probe sets that are being promoted by the CDC to diagnose cases
of COVID-19 infections. Several of these primers have dimerization and
hairpin loop issues, among others. Here, I describe a bioinformatic pipeline to design better
candidate primers that pass stringent design criteria. Using this
pipeline, I generated a new set of primers and probes without the
aforementioned technical issues of the CDC COVID-19 primer probe sets,
and tested their *in silico* precision and recall using the most
recent set of COVID-19 complete genomes from NCBI. The ten best primer-probe pairs have perfect
classification performance (recall, precision, and F1-score all
1.0). I provide these [candidate primers](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/primer-candidate-reports/new-COVID-primer-set-stats_v2.xlsx) for free, under the
[Creative Commons Attribution 4.0 International (CC BY 4.0)
license](https://creativecommons.org/licenses/by/4.0/).

My hope is that this blog post will help start a conversation about the design
process for these critical diagnostic primers. Starting with
these primers, and with input from other scientists, we can develop a
set of candidate primers for COVID-19 that could be used by health
agencies and private diagnostic kit manufacturers alike to not only
generate more diagnostic kits, but *better* ones.

## Primer Design Issues

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

# N1

Here is primer probe set N1, shown in Boulder format (as used by
Primer3):

    SEQUENCE_PRIMER=GACCCCAAAATCAGCGAAAT
    SEQUENCE_INTERNAL_OLIGO=ACCCCGCATTACGTTTGGTGGACC
    SEQUENCE_PRIMER_REVCOMP=TCTGGTTACTGCCAGTTGAATCTG

Here is the formatted analysis summary from Primer3 for N1 (note that
in the terminology of Primer3, the forward primer is termed "Left",
the reverse primer is termed "Right", and the hybridization probe is
termed "Internal Oligo"):

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


# N2

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

# N3

Here is set N3:

    SEQUENCE_PRIMER=GGGAGCCTTGAATACACCAAAA
    SEQUENCE_INTERNAL_OLIGO=ATCACATTGGCACCCGCAATCCTG
    SEQUENCE_PRIMER_REVCOMP=TGTAGCACGATTGCAGCATTG

The forward primer, while having a C base in the last five base positions, has it at the 5' end of that region, with a poly-A tail following. While runs of a single base of one to four base pairs in length are normally tolerated, it's worrisome to place one at the 3' rend. It also had a predicted hairpin loop:

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


## Developing New Candidate Primers

# Primer Design Criteria

In order to design better-performing COVID-19 primers, I developed a
bioinformatic pipeline that generates primers using the following
criteria:

* Target regions conserved perfectly among all COVID complete genomes
  (41 downloaded on 2020-03-02; check [NCBI Nucleotide](https://www.ncbi.nlm.nih.gov/nuccore/?term=txid2697049%5BOrganism%3Anoexp%5D+%22complete+genome%22) for the current list)
* Poly-X runs longer than four bases are not allowed
* Check that hybridization probes do not start with G (avoid quenching)
* Check for GC clamp on the forward and reverse primers (3' end has
  one or two G's or C's in last five base pairs)
* GC% range between 40 and 60, with an optimum at 50
* Primer and probe size range between 18 to 27 bases
* Amplicon size from 75 to 200 bases
* Disallow any hairpins, self-dimers, or oligo-interactions at ANY temperature
* Exclude regions that are identical to [the four known common cold-causing,
  human-associated coronaviruses(229E, NL63, OC43, and HKU1)](https://www.cdc.gov/coronavirus/general-information.html)
* Primers designed to cover the viral RNA polymerase gene, as it tends
  to be highly conserved within an RNA viral species, and different
  from the RNA polymerase sequence of different RNA viral species
  (as per Tom Slezak, former head of biodefense program at
   LLNL, private communication)

# The New Candidate Primers Files

The set of candidate primers (last generated on 2020-03-11) can be downloaded
[here](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/primer-candidate-reports/new-COVID-primer-set-stats_v2.xlsx) in XLSX spreadsheet
format, and [here](https://bitbucket.org/tomeraltman/covid-19-primer-data/src/master/primer-candidate-reports/new-COVID-primer-set-stats_v2.csv) as a
comma-separated value ("CSV") file. These are released as
[Creative Commons Attribution 4.0 International 4.0 (CC BY
4.0)](https://creativecommons.org/licenses/by/4.0/). I recommend
focusing on the top ten primer-probe sets, as they have the greatest
performance (based on the F1-score).

Below find selected details about how the primers were designed and
validated using an 'e-PCR' approach.


# Primer Performance Validation *in silico*

Even if you design your primers using recommended settings with
trusted software like Primer3, there's still the chance that you'll
have off-target binding of the oligos to other stretches of DNA in
your sample, or even off-target amplicons if a forward and reverse
primer bind close enough to one another in the correct
orientation. For that reason, I analyzed the *in silico* performance
of the newly designed primers to determine their predicted recall and
precision. To do so, I performed a sequence homology search using NCBI
Blast+ (version 2.10.0) against the NT database (downloaded
2020-03-02). Complete genomes (whether prokaryotic or viral) where the
forward and reverse primers, and the hybridization probe, all had
gapless alignments with 90% or greater sequence identity, and in the
proper orientation, were counted as a hit. I used all complete viral
genomes in NT with NCBI Taxonomy Database identifier 2697049 (the
identifier for the COVID-19 sequences) as the "gold standard" for
assessing the performance of these primer probe sets as diagnostics
for COVID-19 presence (i.e., these are the sequences that the primers
should hit without fail).

The spreadsheet shows that many of the newly-designed primers have five false negatives, but
this is actually a bug in my current pipeline. It's due to an obscure
detail about how NCBI represents identical genomes in its Blast
databases. Those genomes are all identified, just under a different
identifier. So the top ten primer-probe pairs actually have perfect
performance: precision, recall, and F1-score all 1.0.


## Next Steps

Thank you for reading this far. My hope is that I can start up a
conversation among scientists about how to design and validate
candidate primer probes /in silico/, to allow for faster and more
efficient wet-bench validation of the candidate primers. I hope this
will lead to better, more accurate COVID-19 diagnostic kits being
designed and successfully deployed around the country.

Please feel free to follow up with me via email
(blog-at-me.tomeraltman.net) or Twitter
([@tomeraltman](https://www.twitter.com/tomeraltman)). I look forward
to constructive feedback about how to improve this analysis. I am
working on releasing the source code for the bioinformatic pipeline,
and writing up the methodology in more detail.  Thanks again for your
time, and thanks in advance for any help that you might provide.

TODOs:
* Perform same analysis on WHO recommended COVID-19 primers
* Release software pipeline code
* Complete secondary structure code blocks for low-temperature entries
* Fix false negative reporting bug due to NCBI Blast databases
  **[FIXED: see update post]**
* Prepare technical manuscript detailing the methodology

## Acknowledgments

My deepest gratitude to the following individuals for helping me by
reviewing this post:

* [Tom Slezak](https://www.kpathsci.com/company)
* [Dr. Elisabeth Bik](https://microbiomedigest.com/sample-page/in-the-news/)
* [Dr. Michael Walker](https://www.linkedin.com/in/michael-walker-90aa2452/)
* [Dr. David L. Dill](https://www.linkedin.com/in/david-dill-06b1524)

## Updates

* **[2020-03-06]** Incorporated suggestions and fixes from Nathan Walsh
  and Nora Callahan. Thanks!

* **[2020-03-11]** Released latest version of COVID-19 candidate
    primers. Updated file links. See [this update post](https://tomeraltman.net/2020/03/11/COVID-19-candidate-primers-update.html) for more
    information.
    