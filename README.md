## Empirical Studies
### Usage of Rust
* [Astrauskas et. al OOPSLA'2020][[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMathejaPoliMuellerSummers20.pdf)]: how unsafe code is used
### Classes of Bugs: 
* [Qin et. al PLDI'2020][[PDF](https://cseweb.ucsd.edu/~yiying/RustStudy-PLDI20.pdf)]
 the study indentifies patterns of bugs and offers some insight into their common repairs too, categorising the bugs as:
  * memory-safety bugs: invalid acceses (buffer overflow, null pointer dereferencing, read uninitialized memory) and 
  lifetime violations (invalid free, use after free, double free).
  * thread safety issues: blocking bugs (due to double locking, wrong order of locking - tricky because of Rust's implicit lock release, channel size, codevar),
  non-blocking bugs (data races may occure not only due to unsafe code, but also in safe one)
  * [benchmarks](https://github.com/system-pclub/rust-study): five large open-source applications (Servo---browser, TiKV---key-value store, Parity Ethereum, Redox---OS, Tock---embedded OS) and five widely-used libraries (Rand, Crossbeam, Thredpool, Rayon, Lazy_static)
  * [bug analysis](https://github.com/system-pclub/rust-study#8-bug-detection-section-7) method: mostly manual effrot by inspecting CVEs and commit messages. The work also comes accompanied by an UAF detector (works on MIR) and a double-lock detecor (works on LLVM). 
* [Xu et. al arXiv'2021][[PDF](https://arxiv.org/pdf/2003.03296.pdf)]:
  *  186 memory-safety bugs: buffer overflow, read uninitialized memory, use after free, double free.
  *  [benchmarks](https://github.com/Artisan-Lab/Rust-memory-safety-bugs): Advisory-DB, Trophy Cases, rustc, plus 5 open-source projects 
  *  bug analysis method: manual inspection. 
* 
## Verification
* [Astrauskas et. al OOPSLA'2019][[PDF](https://www.cs.ubc.ca/~alexsumm/papers/AstrauskasMuellerPoliSummers19.pdf)]

## Analysis

## Tool Support


Vytautas Astrauskas, Christoph Matheja, Federico Poli, Peter Müller, and Alexander J. Summers. 2020. How do programmers use unsafe rust? Proc. ACM Program. Lang. 4, OOPSLA, Article 136 (November 2020), 27 pages. DOI:https://doi.org/10.1145/3428204

Vytautas Astrauskas, Peter Müller, Federico Poli, and Alexander J. Summers. 2019. Leveraging rust types for modular specification and verification. Proc. ACM Program. Lang. 3, OOPSLA, Article 147 (October 2019), 30 pages. DOI:https://doi.org/10.1145/3360573

Boqin Qin, Yilun Chen, Zeming Yu, Linhai Song, and Yiying Zhang. 2020. Understanding memory and thread safety practices and issues in real-world Rust programs. In <i>Proceedings of the 41st ACM SIGPLAN Conference on Programming Language Design and Implementation</i> (<i>PLDI 2020</i>). Association for Computing Machinery, New York, NY, USA, 763–779. DOI:https://doi.org/10.1145/3385412.3386036

Hui Xu, Zhuangbin Chen, Mingshen Sun, Yangfan Zhou, Michael Lyu. 2021. Memory-safety challenge considered solved? An in-depth study with all Rust CVEs. arXiv.
