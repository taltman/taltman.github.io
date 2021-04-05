---
layout: post
title: "Train of Thought: Models of Bio-Computation"
---

This morning I read [Scott Aaronson's post musing on the computational
expressiveness of his son's toy train
set](https://www.scottaaronson.com/blog/?p=5402). This immediately
triggered for me a flood of memories about my undergraduate days at
Cal, where Scott and I overlapped for a few years (while he was
younger than me, he was already in grad school). While there, I had
the great fortune of being a researcher in [Adam Arkin's computational
biology lab](https://biosciences.lbl.gov/profiles/adam-p-arkin/),
where I could soak up mind-expanding ideas from the grad students,
post-docs, and staff researchers about biological
computing[^dnacompute], and computing about biology.

One area of interest was in determining what kinds of computation could
be performed by cells or biochemical reaction networks[^turing],[^fsm]. While it has
been shown that some cells have NAND-logic-like behavior[^NAND], it seemed
that this was limiting, because there are only so many genes in a cell
with that kind of behavior, and there are many other tricks up the
sleeve of cells and DNA. It struck me that all of the string
manipulation tricks that a genome can perform might provide a more
expressive form of computation. While I did find one paper[^splicing] that
tackled that directly, I thought that an analogous problem must have
gathered much more attention, and thus there might be a richer vein of
material to mine there.

I thought that the various proteins like RNA polymerase that process
along the strands of DNA in a cell might be viewed as a train moving
along a train track, where "stops" would be analogous to transcribing
a gene[^firecracker]. I thought that all sorts of interesting modifications can
happen to the "track", like inversions. I dug for references online,
and I came across this gem, which uses the common elements of train
track switching to explore the universal computation capabilities of
sufficiently designed train tracks: ["Train Sets" by Adam Chalcraft and Michael
Greene](http://www.monochrom.at/turingtrainterminal/Chalcraft.pdf).

Alas, I lacked the training (no pun intended) to soak up all of this
information and do anything significant with it. Till today I am
fascinated with understanding how the cell meshes "analog" computation
in the form of biochemical signal processing[^sigproc], with the "digital"
computation as expressed in the nucleotide sequence of the
genome.

To find the above link, I had a dusty print-out that I had
miraculously saved from my undergraduate days, but no further
bibliographic information. Searching for the title
and authors, I came across some related pages/articles:

* [Chalcraft-Greene train track automaton](https://esolangs.org/wiki/Chalcraft-Greene_train_track_automaton)
* [Computing With Trains - Turing's Trains](https://www.i-programmer.info/news/112-theory/12067-computing-with-trains-turings-trains.html)
* [Article: Trains of Thought](bit-player.org/wp-content/extras/bph-publications/AmSci-2007-03-Hayes-trains.pdf)

Hopefully someone will find this assemblage of links amusing, or
useful.

[^turing]: [Chemical Kinetics is Turing Universal](https://www.researchgate.net/publication/253809773_Chemical_Kinetics_is_Turing_Universal)
[^fsm]: [Chemical Implementation of Finite-State Machines](https://www.researchgate.net/publication/11743809_Chemical_implementation_of_finite-state_machines)
[^splicing]: [On the Power of Circular Splicing Systems and DNA Computability](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.7963)
[^firecracker]: [Programmable and autonomous computing machine made of biomolecules](https://pubmed.ncbi.nlm.nih.gov/11719800/)
[^dnacompute]: [Recent Developments in DNA-Computing](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.29.9196)
[^NAND]: [Cellular Gate Technology](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.36.4499)
[^sigproc]: [Motifs and Modules in Cellular Signal Processing](https://www.researchgate.net/publication/4033212_Motifs_and_modules_in_cellular_signal_processing_applications_to_microbial_stress_response_pathways)