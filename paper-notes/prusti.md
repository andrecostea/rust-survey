# Prusti
  [[Homepage](https://www.pm.inf.ethz.ch/research/prusti.html)]
  [[GitHub](https://github.com/viperproject/prusti-dev)] 
  [[VS code extension](https://github.com/viperproject/prusti-assistant)] 
  [[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMuellerPoliSummers19.pdf)]
  
Prusti is an automatic verifier for Rust code which works on MIR. In a nutshell, Prusti: 
* extracts the CFG representation of Rust code, translating each Rust function to a Viper method, each Rust pure function to a Viper function, and
* collects the type information provided by `rustc`, translating it into specifications written in [Viper's intermediate verification language](https://doi.org/10.1007/978-3-662-49122-5_2) (each Rust type will be a Viper predicate), and, finally, 
* verifies that the translated source code is a refinement of the ascribed specification---synthesise a core proof, and
* translates the verification results from Viper back to Rust, reporting them via Rust compiler’s error reporting mechanisms. 

Additionally to the above guarantees, namely that every type-checked program is successfully verified, Prusti also allows programmers to write _functional specifications_ which complement the type-safe information to offer stronger correctness guarantees. 

Prusti, relying on Viper, synthesise a core proof of _memory safety_ (exclusive capability for each mutable memory location, shared capability in the presence of multiple aliases to the same memory location) of the program in a flavour of separation logic: [implicit dynamic frames](https://doi.org/10.1007/978-3-642-03013-0_8).  


Prusti ran 3 kinds of *experiments*:
* large scale one---verified over 10k functions from open source crates---to confirm that rustc is indeed correct. Whatever is compiled by rustc, gets verified by Prusti too. (a sort of sanity check for the compiler). Not sure whether the word large scale is appropriate though. It refers to a large number of small functions.
* medium scale---checked over 500 functions for integer overflows and divisions by zero. Interestingly, 467 have had errors because of assumptions over the arguments, assumptions which have not been enforced. 
* small scale---11 toy examples of max 80 LOC where they highlighted the functionality of the correctness specs provided by the user. 

TODO: supported language features, unsupported language features (unsafe code)

## Thoughts on APR:

While Prusti is a great piece of work with its fully automatic approach, it _does not handle unsafe_ code. That immediately excludes a large class of bugs we could tackle---memory bugs, concurrency bugs---which mostly emerge when a combination of safe and unsafe code is used. 

We could tap into its bugs detection mechanisms for integer overflows and divisions by zero. This part is fully automatic, needs no user input, and should be amenable to APR.

Assuming the existence of an oracle which writes specs, we could also harness the functional specifications to repair programs which may be safe, but computationally incorrect.
