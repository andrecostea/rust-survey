# Prusti
  [[Homepage](https://www.pm.inf.ethz.ch/research/prusti.html)]
  [[GitHub](https://github.com/viperproject/prusti-dev)] 
  [[VS code extension](https://github.com/viperproject/prusti-assistant)] 
  [[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMuellerPoliSummers19.pdf)]
  
Prusti is an automatic verifier for Rust code which works on MIR. In a nutshell, 

Prusti ran 3 kinds of experiments:
* large scale one---verified over 10k functions from open source crates---to confirm that rustc is indeed correct. Whatever is compiled by rustc, gets verified by Prusti too. (a sort of sanity check for the compiler). Not sure whether the word large scale is appropriate though. It refers to a large number of small functions.
* medium scale---checked over 500 functions for integer overflows and divisions by zero. Interestingly, 467 have had errors because of assumptions over the arguments, assumptions which have not been enforced. If we are keen to use Prusti for APR, this could be the only class of bugs we can focus on. I assume it would be cool though to repair 467 functions!
* small scale---11 toy examples of max 80 LOC where they highlighted the functionality of the correctness specs provided by the user. I suppose there is some space here too, where we assume an oracle to describe what correctness means.


## Thoughts on APR:

While Prusti is a great piece of work with its fully automatic approach, it _does not handle unsafe_ code. That immediately excludes a large class of bugs we could tackle---memory bugs, concurrency bugs---which mostly emerge when a combination of safe and unsafe code is used. 
