# Rust

&emsp;&emsp; .rs    
---> AST (macro expansion)   
---> HIR (type checking)   
---> MIR (borrow checker & lifetime inference, optimizations; CFG)  
---> LLVM    
---> exec  

## Empirical Studies

### Usage of Rust
* [Astrauskas et. al OOPSLA'20][[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMathejaPoliMuellerSummers20.pdf)]: how unsafe code is used. Concretely, it aims to answer the following questions:
     * (Frequency) How often does unsafe code appear explicitly in Rust crates?
     * (Size) What is the size of unsafe blocks that programmers write? R: typically small blocks, 75% of all unsafe blocks
comprise at most 21 #MIR.
     * (Self-containedness) Is the behaviour of unsafe code dependent only on code in its own crate?
     * (Encapsulation). Is unsafe code typically shielded from clients through safe abstractions?
     * (Motivation). What are the most prevalent use cases for unsafe code? R: call to unsafe function,
dereference of raw pointer,
use of mutable static variable, 
access to union field,
use of extern static variable,
use of inline assembly, ...
* [Mehmet et. al OOPSLA'21] Sources of unsafety in code automatically translated by [c2rust](https://c2rust.com/) - [See below](#code-transpiling-and-refactoring)
* [Evans et. al ICSE'20] [[PDF](https://arxiv.org/pdf/2007.00752.pdf)]: how unsafe code is used. Aims to answer the following questions:
     * How much do developers use Unsafe Rust?
     * How much of the Rust code is Unsafe Rust? 
     * What Unsafe Rust operations are used in practice?
     * What abstract binary interfaces (programming languages) are used in the declared unsafe functions?
     * Does the use of Unsafe Rust change over time?
     * Why do Rust developers use unsafe?

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
* RustHorn [[GitHub](https://github.com/hopv/rust-horn)] 
           [[PDF](https://arxiv.org/pdf/2002.09002.pdf)]
           [Yusuke et. al PLS'20]: uses Horn clauses to verify partial correctness. 
* Creusot [[GitHub]https://github.com/xldenis/creusot]: deductive verification of Rust code; allows annotations. 

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
 * RUDRA [[GitHub](https://github.com/sslab-gatech/Rudra)][[PDF](https://taesoo.kim/pubs/2021/bae:rudra.pdf)] - used for detecting potential memory safety bugs in unsafe Rust (Panic Safety, Higher Order invariant, Send/Sync VAriance). They managed to run it on the entire Rust package registry  (a [list](https://github.com/sslab-gatech/Rudra-PoC) of all detected bugs). It is now integrated into the official Rust linter.  

### Dynamic
 * MIRI [[GitHub](https://github.com/rust-lang/miri)] - interpreter for MIR, can detect certain classes of memory errors and data races. It aslo supports [Stacked Borrows](https://plv.mpi-sws.org/rustbelt/stacked-borrows/) to check for violation of pointer aliasing discipline in unsafe code. 

## Synthesis
  * RUSSOL
    [[PDF](https://ilyasergey.net/assets/pdf/papers/russol-pldi23.pdf)]
    [[Artefact](https://zenodo.org/record/7811786)] -  synthesizing correct-by-construction programs in safe Rust from functional correctness specifications.
## Testing
  * RMC [[GitHub](https://github.com/model-checking/rmc)] [[Post](https://whileydave.com/2021/10/26/test-driving-the-rust-model-checker-rmc/)] -  CBMC for Rust. Work on converting MIR to the input language of CBMC. #mir
  * SyRust [[PDF](https://kilthub.cmu.edu/articles/report/SyRust_Automatic_Testing_of_Rust_Libraries_with_Semantic-Aware_Program_Synthesis_Technical_Report/14356949)]

## Code transpiling and refactoring
  * [`c2rust`](https://c2rust.com/) -  translates C code to unsafe Rust
  * [Zhang et. al CAV'23]
    [[PDF](https://arxiv.org/abs/2303.10515)] - automated C to Rust translation grounded in static ownership analysis.
  * [Mehmet et. al OOPSLA'23]
    [[PDF](https://dl.acm.org/doi/10.1145/3485498)]
    [[Artefact](https://zenodo.org/record/7714175)] - transaltes C to Rust via `c2rust`, and builds on top of [`laertes`](https://dl.acm.org/doi/pdf/10.1145/3485498) now handling pointer arithmetic too.
  * [Han et al. QRS-C'22]
    [[PDF](https://csslab-ustc.github.io/publications/2023/rust-dup.pdf)] - Rusty
  * [Mehmet et. al OOPSLA'21]
    [[PDF](https://dl.acm.org/doi/pdf/10.1145/3485498)] 
    [[Artefact - laertes](https://zenodo.org/record/5442253#.YXoVchBBxTY)] 
    [[Translation Rules](https://sites.cs.ucsb.edu/~benh/research/papers/oopsla21-supplementary.pdf)] - translates C to unsafe Rust and lifts raw pointers to Optional references (in order to maintain the C interface)
  * [Sam et. al ACSW'17] 
    [[PDF](https://homepages.ecs.vuw.ac.nz/~alex/files/SamCameronPotaninACSC2017.pdf)] 
    [[GitHub](https://github.com/GSam/rust-refactor)] - syntactic changes (variable renaming, inlining, lifetime elission from functio signature)
  * [Zborowski'17]  [[PDF](https://homepages.cwi.nl/~jurgenv/theses/AdrianZborowski.pdf)] - refactoring rust output from the outdated `corrode` c-to-rust translator; loops, and basic ownership translation.

## Semantics Formalization
  * Oxide [[PDF](https://arxiv.org/pdf/1903.00982)] [[Slides](https://aaronweiss.us/pubs/popl19-src-oxide-slides.pdf)] [Weiss et. al arXiv'19]
  * KRust [[PDF](https://arxiv.org/pdf/1804.10806)] [Want et. al TASE'18]
  * RustSEM [[PDF](https://arxiv.org/pdf/1804.07608.pdf)] [Kan et. al arXiv'18]

## Mitigation Solutions
  * [Liu et. al ICSE'20] [PDF](https://peimingliu.github.io/asset/pic/icse-paper1026.pdf) -  ensures data flow integrity between unsafe Rust code and safe Rust code by isolating the footprint of unsafe Rust from the footprint of safe Rust (prevents any cross-region
memory corruption).

## Tool Support
* [`clippy`](https://github.com/rust-lang/rust-clippy) - offers a collection of existing lints and an interface to easily [[create a custom lint](https://github.com/rust-lang/rust-clippy/blob/master/doc/adding_lints.md)]. The lints work on ASTs at HIR level. 
* [`rustfix`](https://github.com/rust-lang/rustfix) - is a library which takes as input a set of edits (generated by `rustc` and/or `clippy`) and applies them. This is achieved via `cargo fix` - a tool which automatically invokes `rustc`, collects the suggestions in JSON format, runs `rustfix` to apply the edits, and finally checks that the resulted code still compiles.
* [`rust-analysis`](https://hackmd.io/RiztubvfT4eOk4-4nM8Y7Q?view) [[Video](https://www.youtube.com/watch?v=SKmd5A-1cSE)]- aims to support customized analysis (still a WIP though)
* RustViz (https://github.com/rustviz/rustviz)[[PDF](https://web.eecs.umich.edu/~comar/rustviz-hatra20.pdf)]: tool to visualize the ownership and borrowing

## Check these blogs, presentations, reports: 
* rust-lang development: http://smallcultfollowing.com/babysteps/
* How can we use the compiler's interanls for building analysis tools: https://hackmd.io/RiztubvfT4eOk4-4nM8Y7Q?view
* On StackBorrows/RustBelt/MIRI and others: https://www.ralfj.de/blog
* Tools and applications: https://github.com/rust-unofficial/awesome-rust
* A list of verifications tools: https://alastairreid.github.io/rust-verification-tools/
* A Guideline for Unsafe Code: https://github.com/rust-lang/unsafe-code-guidelines
* A [report](https://arxiv.org/pdf/2011.06171.pdf) on understanding ownership and borrowing when teaching Rust




## References

Vytautas Astrauskas, Christoph Matheja, Federico Poli, Peter Müller, and Alexander J. Summers. 2020. How do programmers use unsafe rust? Proc. ACM Program. Lang. 4, OOPSLA, Article 136 (November 2020), 27 pages. DOI:https://doi.org/10.1145/3428204

Vytautas Astrauskas, Peter Müller, Federico Poli, and Alexander J. Summers. 2019. Leveraging rust types for modular specification and verification. Proc. ACM Program. Lang. 3, OOPSLA, Article 147 (October 2019), 30 pages. DOI:https://doi.org/10.1145/3360573

Fabian Wolff, Aurel Bílý, Christoph Matheja, Peter Müller, and Alexander J. Summers. 2021. Modular Specification and Verification of Closures in Rust. In Proc. ACM Program. Lang., Vol. 5, No. OOPSLA, Article 145. Publication date: October 2021

Zhang, H., David, C., Yu, Y., & Wang, M. (2023). Ownership guided C to Rust translation. ArXiv, abs/2303.10515.

Mohan Cui, Chengjun Chen, Hui Xu, Yangfan Zhou. 2021. SafeDrop: Detecting Memory Deallocation Bugs of Rust Programs via Static Data-Flow Analysis. arVix.

Ana Nora Evans, Bradford Campbell, and Mary Lou Soffa. 2020. Is rust used safely by software developers? In <i>Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering</i> (<i>ICSE '20</i>). Association for Computing Machinery, New York, NY, USA, 246–257. DOI:https://doi.org/10.1145/3377811.3380413

Zhijian Huang, Yong Jun Wang, Jing Liu. 2018. Detecting Unsafe Raw Pointer Dereferencing Behavior in Rust. IEICE Trans. Inf. Syst., 101-D, 2150-2153.

Ralf Jung, Jacques-Henri Jourdan, Robbert Krebbers, and Derek Dreyer. 2017. RustBelt: securing the foundations of the Rust programming language. Proc. ACM Program. Lang. 2, POPL, Article 66 (January 2018), 34 pages. DOI:https://doi.org/10.1145/3158154

Shuanglong Kan, David Sanán, Shang-Wei Lin, Yang Liu: "K-Rust: An Executable Formal Semantics for Rust". CoRR abs/1804.07608 (2018)

Peiming Liu, Gang Zhao, and Jeff Huang. 2020. Securing unsafe rust programs with XRust. In Proceedings of the ACM/IEEE 42nd International Conference on Software Engineering (ICSE '20). Association for Computing Machinery, New York, NY, USA, 234–245. DOI:https://doi.org/10.1145/3377811.3380325

Mehmet Emre, Ryan Schroeder, Kyle Dewey, and Ben Hardekopf. 2021. Translating C to safer Rust. <i>Proc. ACM Program. Lang.</i> 5, OOPSLA, Article 121 (October 2021), 29 pages. DOI:https://doi.org/10.1145/3485498

Mehmet Emre, Peter Boyland, Aesha Parekh, Ryan Schroeder, Kyle Dewey, and Ben Hardekopf. 2023. Aliasing Limits on Translating C to Safe Rust. Proc. ACM Program. Lang. 7, OOPSLA1, Article 94 (April 2023), 29 pages. https://doi.org/10.1145/3586046

X. Han, B. Hua, Y. Wang and Z. Zhang, "RUSTY: Effective C to Rust Conversion via Unstructured Control Specialization," 2022 IEEE 22nd International Conference on Software Quality, Reliability, and Security Companion (QRS-C), Guangzhou, China, 2022, pp. 760-761, doi: 10.1109/QRS-C57518.2022.00122.

Garming Sam, Nick Cameron, and Alex Potanin. 2017. Automated refactoring of rust programs. In Proceedings of the Australasian Computer Science Week Multiconference (ACSW '17). Association for Computing Machinery, New York, NY, USA, Article 14, 1–9. DOI:https://doi.org/10.1145/3014812.3014826

Joshua Yanovski, Hoang-Hai Dang, Ralf Jung, and Derek Dreyer. 2021. GhostCell: Separating Permissions from Data in Rust. In Proc. ACM Program. Lang., Vol. 5, No. ICFP, Article 92. Publication date: August 2021. https://dl.acm.org/doi/10.1145/3473597

Gongming, & Luo, & Reddy, Vishnu & Almeida, Marcelo & Zhu, Yingying & Du, Ke & Omar, Cyrus. (2020). RustViz: Interactively Visualizing Ownership and Borrowing. 

Boqin Qin, Yilun Chen, Zeming Yu, Linhai Song, and Yiying Zhang. 2020. Understanding memory and thread safety practices and issues in real-world Rust programs. In <i>Proceedings of the 41st ACM SIGPLAN Conference on Programming Language Design and Implementation</i> (<i>PLDI 2020</i>). Association for Computing Machinery, New York, NY, USA, 763–779. DOI:https://doi.org/10.1145/3385412.3386036

F. Wang, F. Song, M. Zhang, X. Zhu and J. Zhang, "KRust: A Formal Executable Semantics of Rust," 2018 International Symposium on Theoretical Aspects of Software Engineering (TASE), 2018, pp. 44-51, doi: 10.1109/TASE.2018.00014.

Aaron Weiss, Daniel Patterson, Nicholas D. Matsakis, Amal Ahmed: "Oxide: The Essence of Rust" CoRR abs/1903.00982 (2019)

Hui Xu, Zhuangbin Chen, Mingshen Sun, Yangfan Zhou, Michael Lyu. 2021. Memory-safety challenge considered solved? An in-depth study with all Rust CVEs. arXiv.

Zborowski, Adrian. "Framework for Idiomatic Refactoring of Rust Programming Language Code."

Matsushita, Yusuke, Takeshi Tsukada and Naoki Kobayashi. “RustHorn: CHC-Based Verification for Rust Programs.” Programming Languages and Systems 12075 (2020): 484 - 514.

