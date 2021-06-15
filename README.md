## Empirical Studies

### Usage of Rust
* [Astrauskas et. al OOPSLA'2020][[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMathejaPoliMuellerSummers20.pdf)]: how unsafe code is used

### Classes of Bugs: 
* [Qin et. al PLDI'2020]
  [[PDF](https://cseweb.ucsd.edu/~yiying/RustStudy-PLDI20.pdf)]
  [[Benchmarks](https://github.com/system-pclub/rust-study)]
  the study indentifies patterns of bugs and offers some insight into their common repairs too, distinguising between memory safety    issues and concurrency specific ones.

* [Xu et. al arXiv'2021][[PDF](https://arxiv.org/pdf/2003.03296.pdf)]:
  *  186 memory-safety bugs: buffer overflow, read uninitialized memory, use after free, double free.
  *  [benchmarks](https://github.com/Artisan-Lab/Rust-memory-safety-bugs): Advisory-DB, Trophy Cases, rustc, plus 5 open-source projects 
  *  bug analysis method: manual inspection. 

## Verification
### Safe Code
* Prusti 
  [[Homepage](https://www.pm.inf.ethz.ch/research/prusti.html)]
  [[GitHub](https://github.com/viperproject/prusti-dev)] 
  [[VS code extension](https://github.com/viperproject/prusti-assistant)] 
  [[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMuellerPoliSummers19.pdf)]
  [[notes](paper-notes/prusti.md) on Astrauskas et. al OOPSLA'2019] - translates Rust programs
    and specifications into the intermediate language used by [Viper](https://www.pm.inf.ethz.ch/research/viper.html). 
    Verifies the translated program for memory safety using the information provided by Rust's type system, and for functional correctness by adding support for user provided specification in Rust programs. #separation-logic #mir  

### Unsafe Code
* RustBelt [[Homepage](https://plv.mpi-sws.org/rustbelt/)]
  [[PDF](https://people.mpi-sws.org/~dreyer/papers/rustbelt/paper.pdf)]
  [[notes](paper-notes/rustbelt.md) on Jung et. al POPL'2018] - machine checked safety proofs for a subset of Rust. In particular, it      derives the sufficient safety conditions for libraries using unsafe code to be deemed as safe. It works on lambda-rust, a language similar to MIR, hence RustBelt still needs a human in the loop to do the translation from MIR to lambda-rust. 
  #separation-logic #mir #coq 

## Analysis
### Static

 * SafeDrop [[PDF](https://arxiv.org/pdf/2103.15420.pdf)] [Cui et. al arVix'2021]
 * UnsafeFencer [[GitHub](https://github.com/qorost/unsafefencer)][[PDF](https://www.jstage.jst.go.jp/article/transinf/E101.D/8/E101.D_2018EDL8040/_pdf)][Huang et.al 2018]

### Dynamic

## Tool Support


Vytautas Astrauskas, Christoph Matheja, Federico Poli, Peter Müller, and Alexander J. Summers. 2020. How do programmers use unsafe rust? Proc. ACM Program. Lang. 4, OOPSLA, Article 136 (November 2020), 27 pages. DOI:https://doi.org/10.1145/3428204

Vytautas Astrauskas, Peter Müller, Federico Poli, and Alexander J. Summers. 2019. Leveraging rust types for modular specification and verification. Proc. ACM Program. Lang. 3, OOPSLA, Article 147 (October 2019), 30 pages. DOI:https://doi.org/10.1145/3360573

Mohan Cui, Chengjun Chen, Hui Xu, Yangfan Zhou. 2021. SafeDrop: Detecting Memory Deallocation Bugs of Rust Programs via Static Data-Flow Analysis. arVix.

Zhijian Huang, Yong Jun Wang, Jing Liu. 2018. Detecting Unsafe Raw Pointer Dereferencing Behavior in Rust. IEICE Trans. Inf. Syst., 101-D, 2150-2153.

Ralf Jung, Jacques-Henri Jourdan, Robbert Krebbers, and Derek Dreyer. 2017. RustBelt: securing the foundations of the Rust programming language. Proc. ACM Program. Lang. 2, POPL, Article 66 (January 2018), 34 pages. DOI:https://doi.org/10.1145/3158154

Boqin Qin, Yilun Chen, Zeming Yu, Linhai Song, and Yiying Zhang. 2020. Understanding memory and thread safety practices and issues in real-world Rust programs. In <i>Proceedings of the 41st ACM SIGPLAN Conference on Programming Language Design and Implementation</i> (<i>PLDI 2020</i>). Association for Computing Machinery, New York, NY, USA, 763–779. DOI:https://doi.org/10.1145/3385412.3386036

Hui Xu, Zhuangbin Chen, Mingshen Sun, Yangfan Zhou, Michael Lyu. 2021. Memory-safety challenge considered solved? An in-depth study with all Rust CVEs. arXiv.

