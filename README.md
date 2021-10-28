## Empirical Studies

### Usage of Rust
* [Astrauskas et. al OOPSLA'2020][[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMathejaPoliMuellerSummers20.pdf)]: how unsafe code is used'
* [Mehmet et. al OOPSLA'21] Sources of unsafety in code automatically translated by [c2rust](https://c2rust.com/) - [See below](#code-transpiling-and-refactoring)

### Classes of Bugs: 
* [Qin et. al PLDI'2020]
  [[PDF](https://cseweb.ucsd.edu/~yiying/RustStudy-PLDI20.pdf)]
  [[Benchmarks](https://github.com/system-pclub/rust-study)]
  [[Notes](paper-notes/bugs-pldi2020.md)]
  indentifies patterns of bugs and offers some insight into their common repairs too, distinguising between memory safety issues and concurrency specific ones.
   #unsafe #memorybugs #concurrency

* [Xu et. al arXiv'2021]
  [[PDF](https://arxiv.org/pdf/2003.03296.pdf)]
  [[Benchmarks](https://github.com/Artisan-Lab/Rust-memory-safety-bugs)]
  [[Notes](paper-notes/bugs-arxiv2021.md)] focuses on memory-safety bugs.
  #unsafe #memorybugs
  
## Verification
### Safe Code
* Prusti 
  [[Homepage](https://www.pm.inf.ethz.ch/research/prusti.html)]
  [[GitHub](https://github.com/viperproject/prusti-dev)] 
  [[VS code extension](https://github.com/viperproject/prusti-assistant)] 
     * [[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMuellerPoliSummers19.pdf)]
  [[notes](paper-notes/prusti.md) on Astrauskas et. al OOPSLA'19] - translates Rust programs
    and specifications into the intermediate language used by [Viper](https://www.pm.inf.ethz.ch/research/viper.html). 
    Verifies the translated program for memory safety using the information provided by Rust's type system, and for functional correctness by adding support for user provided specification in Rust programs. #separation-logic #mir  
    * [[PDF](https://www.cs.ubc.ca/~alexsumm/papers/WolffBilyMathejaMuellerSummers21.pdf)]
  [[notes](paper-notes/prusti.md) on Wolff et. al OOPSLA'21] - specification and verification of closure-manipulating
code in Rust in the presence of side effects on closure arguments and captured state. It relies on Rust's type system for deriving a strong framing guarantee, namely that the only mutation allowed on a captured state is by the closure itself. 

### Unsafe Code
* RustBelt [[Homepage](https://plv.mpi-sws.org/rustbelt/)]
     * [[PDF](https://people.mpi-sws.org/~dreyer/papers/rustbelt/paper.pdf)]
    [[notes](paper-notes/rustbelt.md) on Jung et. al POPL'18] - machine checked safety proofs for a subset of Rust. In particular, it      derives the sufficient safety conditions for libraries using unsafe code to be deemed as safe. It works on lambda-rust, a language similar to MIR, hence RustBelt still needs a human in the loop to do the translation from MIR to lambda-rust. 
    #separation-logic #mir #coq 
    * [[PDF](http://plv.mpi-sws.org/rustbelt/ghostcell/paper.pdf)]
    [[notes](paper-notes/rustbelt.md) on Yanovski et. al ICFP'21] - proposes a new Rust API, namely GhostCell, to safely implement data structures with internal sharing. The key enabler of this API is the separation of permissions from data, i.e. tracks permissions
separately from the data they control.

## Analysis
### Static
 * MIRAI [[GitHub](https://github.com/facebookexperimental/MIRAI)]
   [[Detailed description](https://github.com/facebookexperimental/MIRAI/blob/master/documentation/Overview.md)] 
   [[Video](https://www.youtube.com/watch?v=vMGilPbIotw)]- interpreter that supports both concrete and abstract values from the interval and tag domains. (the classes of bugs are unclear to me as of now, but I suspect it's only taint analysis)
   #mir #unsafe
 * RMC [[GitHub]https://github.com/model-checking/rmc)] - Rust Model Checker (not much info - need to contact authors)
 * SMACK [[GitHub](https://github.com/smackers/smack)]
  [[Video](https://www.youtube.com/watch?v=0DcIn7kiNxM)] - relies on LLVM IR and Boogie IVL to verify Rust code. 
  #llvm
 * SafeDrop [[PDF](https://arxiv.org/pdf/2103.15420.pdf)] [Cui et. al arVix'2021]
 * UnsafeFencer [[GitHub](https://github.com/qorost/unsafefencer)][[PDF](https://www.jstage.jst.go.jp/article/transinf/E101.D/8/E101.D_2018EDL8040/_pdf)][Huang et.al 2018]

### Dynamic
 * MIRI [[GitHub](https://github.com/rust-lang/miri)] - interpreter for MIR, can detect certain classes of memory errors and data races. It aslo supports [Stacked Borrows](https://plv.mpi-sws.org/rustbelt/stacked-borrows/) to check for violation of pointer aliasing discipline in unsafe code. 

## Testing
  * RMC [[GitHub](https://github.com/model-checking/rmc)] [[Post](https://whileydave.com/2021/10/26/test-driving-the-rust-model-checker-rmc/)] -  CBMC for Rust. Work on converting MIR to the input language of CBMC. #mir
  * SyRust [[PDF](https://kilthub.cmu.edu/articles/report/SyRust_Automatic_Testing_of_Rust_Libraries_with_Semantic-Aware_Program_Synthesis_Technical_Report/14356949)]

## Code transpiling and refactoring
  * [`c2rust`](https://c2rust.com/) -  translates C code to unsafe Rust
  * [Mehmet et. al OOPSLA'21]
    [[PDF](https://dl.acm.org/doi/pdf/10.1145/3485498)] 
    [[Artefact](https://zenodo.org/record/5442253#.YXoVchBBxTY)] 
    [[Translation Rules](https://sites.cs.ucsb.edu/~benh/research/papers/oopsla21-supplementary.pdf)] - translates C to unsafe Rust and lifts raw pointers to Optional references (in order to maintain the C interface)
  * [Sam et. al ACSW'17] 
    [[PDF](https://homepages.ecs.vuw.ac.nz/~alex/files/SamCameronPotaninACSC2017.pdf)] 
    [[GitHub](https://github.com/GSam/rust-refactor)] - syntactic changes (variable renaming, inlining, lifetime elission from functio signature)
  
## Semantics Formalization
  * Oxide [[PDF](https://arxiv.org/pdf/1903.00982)] [[Slides](https://aaronweiss.us/pubs/popl19-src-oxide-slides.pdf)] [Weiss et. al arXiv'19]
  * KRust [[PDF](https://arxiv.org/pdf/1804.10806)] [Want et. al TASE'18]
  * RustSEM [[PDF](https://arxiv.org/pdf/1804.07608.pdf)] [Kan et. al arXiv'18]

## Tool Support
* [`clippy`](https://github.com/rust-lang/rust-clippy) - offers a collection of existing lints and an interface to easily [[create a custom lint](https://github.com/rust-lang/rust-clippy/blob/master/doc/adding_lints.md)]. The lints work on ASTs at HIR level. 
* [`rustfix`](https://github.com/rust-lang/rustfix) - is a library which takes as input a set of edits (generated by `rustc` and/or `clippy`) and applies them. This is achieved via `cargo fix` - a tool which automatically invokes `rustc`, collects the suggestions in JSON format, runs `rustfix` to apply the edits, and finally checks that the resulted code still compiles.
* [`rust-analysis`](https://hackmd.io/RiztubvfT4eOk4-4nM8Y7Q?view) [[Video](https://www.youtube.com/watch?v=SKmd5A-1cSE)]- aims to support customized analysis (still a WIP though)

## Check these blogs and presentations 
* rust-lang development: http://smallcultfollowing.com/babysteps/
* How can we use the compiler's interanls for building analysis tools: https://hackmd.io/RiztubvfT4eOk4-4nM8Y7Q?view
* On StackBorrows/RustBelt/MIRI and others: https://www.ralfj.de/blog
* Tools and applications: https://github.com/rust-unofficial/awesome-rust
* A list of verifications tools: https://alastairreid.github.io/rust-verification-tools/
* A Guideline for Unsafe Code: https://github.com/rust-lang/unsafe-code-guidelines



## References

Vytautas Astrauskas, Christoph Matheja, Federico Poli, Peter Müller, and Alexander J. Summers. 2020. How do programmers use unsafe rust? Proc. ACM Program. Lang. 4, OOPSLA, Article 136 (November 2020), 27 pages. DOI:https://doi.org/10.1145/3428204

Vytautas Astrauskas, Peter Müller, Federico Poli, and Alexander J. Summers. 2019. Leveraging rust types for modular specification and verification. Proc. ACM Program. Lang. 3, OOPSLA, Article 147 (October 2019), 30 pages. DOI:https://doi.org/10.1145/3360573

Fabian Wolff, Aurel Bílý, Christoph Matheja, Peter Müller, and Alexander J. Summers. 2021. Modular Specification and Verification of Closures in Rust. In Proc. ACM Program. Lang., Vol. 5, No. OOPSLA, Article 145. Publication date: October 2021

Mohan Cui, Chengjun Chen, Hui Xu, Yangfan Zhou. 2021. SafeDrop: Detecting Memory Deallocation Bugs of Rust Programs via Static Data-Flow Analysis. arVix.

Zhijian Huang, Yong Jun Wang, Jing Liu. 2018. Detecting Unsafe Raw Pointer Dereferencing Behavior in Rust. IEICE Trans. Inf. Syst., 101-D, 2150-2153.

Ralf Jung, Jacques-Henri Jourdan, Robbert Krebbers, and Derek Dreyer. 2017. RustBelt: securing the foundations of the Rust programming language. Proc. ACM Program. Lang. 2, POPL, Article 66 (January 2018), 34 pages. DOI:https://doi.org/10.1145/3158154

Shuanglong Kan, David Sanán, Shang-Wei Lin, Yang Liu: "K-Rust: An Executable Formal Semantics for Rust". CoRR abs/1804.07608 (2018)

Mehmet Emre, Ryan Schroeder, Kyle Dewey, and Ben Hardekopf. 2021. Translating C to safer Rust. <i>Proc. ACM Program. Lang.</i> 5, OOPSLA, Article 121 (October 2021), 29 pages. DOI:https://doi.org/10.1145/3485498

Garming Sam, Nick Cameron, and Alex Potanin. 2017. Automated refactoring of rust programs. In Proceedings of the Australasian Computer Science Week Multiconference (ACSW '17). Association for Computing Machinery, New York, NY, USA, Article 14, 1–9. DOI:https://doi.org/10.1145/3014812.3014826

Joshua Yanovski, Hoang-Hai Dang, Ralf Jung, and Derek Dreyer. 2021. GhostCell: Separating Permissions from Data in Rust. In Proc. ACM Program. Lang., Vol. 5, No. ICFP, Article 92. Publication date: August 2021. https://dl.acm.org/doi/10.1145/3473597

Boqin Qin, Yilun Chen, Zeming Yu, Linhai Song, and Yiying Zhang. 2020. Understanding memory and thread safety practices and issues in real-world Rust programs. In <i>Proceedings of the 41st ACM SIGPLAN Conference on Programming Language Design and Implementation</i> (<i>PLDI 2020</i>). Association for Computing Machinery, New York, NY, USA, 763–779. DOI:https://doi.org/10.1145/3385412.3386036

F. Wang, F. Song, M. Zhang, X. Zhu and J. Zhang, "KRust: A Formal Executable Semantics of Rust," 2018 International Symposium on Theoretical Aspects of Software Engineering (TASE), 2018, pp. 44-51, doi: 10.1109/TASE.2018.00014.

Aaron Weiss, Daniel Patterson, Nicholas D. Matsakis, Amal Ahmed: "Oxide: The Essence of Rust" CoRR abs/1903.00982 (2019)

Hui Xu, Zhuangbin Chen, Mingshen Sun, Yangfan Zhou, Michael Lyu. 2021. Memory-safety challenge considered solved? An in-depth study with all Rust CVEs. arXiv.



