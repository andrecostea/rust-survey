  * 50 memory-safety bugs: invalid acceses (buffer overflow, null pointer dereferencing, read uninitialized memory) and 
  lifetime violations (invalid free, use after free, double free).
  * thread safety issues: 59 blocking bugs (due to double locking, wrong order of locking - tricky because of Rust's implicit lock release, channel size, codevar),
  41 non-blocking bugs (data races may occure not only due to unsafe code, but also in safe one)
  * [benchmarks](https://github.com/system-pclub/rust-study): five large open-source applications (Servo---browser, TiKV---key-value store, Parity Ethereum, Redox---OS, Tock---embedded OS) and five widely-used libraries (Rand, Crossbeam, Thredpool, Rayon, Lazy_static)
  * [bug analysis](https://github.com/system-pclub/rust-study#8-bug-detection-section-7) method: mostly manual effrot by inspecting CVEs and commit messages. The work also comes accompanied by an UAF detector (works on MIR) and a double-lock detecor (works on LLVM). 
