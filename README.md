# Repository Name Suggestion: `auto-adaptive-nuclei-scanner`

**Description (for GitHub repo creation):**

> A robust, adaptive Bash script for automated subdomain scanning and vulnerability assessment using Nuclei and HTTPX. Automatically detects system resources and tunes scanning parameters for optimal performance on any Linux system.

---

# Auto-Adaptive Nuclei Scanner (Robust Version)

![Bash](https://img.shields.io/badge/Language-Bash-informational) ![License](https://img.shields.io/badge/License-MIT-blue)

A robust, adaptive Bash script for automated subdomain scanning and vulnerability assessment using **Nuclei** and **HTTPX**. The script automatically detects system resources (CPU, RAM, disk, GPU) and adapts scanning parameters for optimal performance on any Linux machine.

---

## Features

* ✅ Automatic system detection:

  * CPU threads
  * Available RAM
  * Disk free space
  * NVIDIA GPU (if available)
* ✅ Adaptive scanning:

  * Dynamic chunk size based on available RAM
  * Concurrency and rate limits based on CPU threads
* ✅ HTTPX integration for live host filtering
* ✅ Nuclei scanning with retry mechanism
* ✅ Verbose logging option
* ✅ Custom scan folder and output logging
* ✅ Safe fallback for minimal systems (commands like `nproc`, `free`, `df`, or `nvidia-smi` may be missing)
* ✅ Color-coded console output

---

## Requirements

* Linux OS (tested on Kali, Ubuntu)
* [Nuclei](https://nuclei.projectdiscovery.io/)
* [HTTPX](https://github.com/projectdiscovery/httpx) (optional, for live host filtering)
* Bash 4+
* Standard Linux utilities: `awk`, `grep`, `split`
* Optional utilities: `nproc`, `free`, `df`, `nvidia-smi`

---

## Installation

1. **Clone the repository**:

```bash
git clone https://github.com/yourusername/auto-adaptive-nuclei-scanner.git
cd auto-adaptive-nuclei-scanner
```

2. **Make the script executable**:

```bash
chmod +x auto_nuclei.sh
```

3. **Ensure Nuclei templates exist**:

```bash
mkdir -p ~/nuclei-templates
nuclei -update-templates
```

4. **Install HTTPX** (optional):

```bash
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
```

---

## Usage

```bash
./auto_nuclei.sh -f subs.txt [-x run|httpx.txt] [-v] [-d scan_dir] [-l log.txt]
```

### Options

| Option | Description                                                                        |
| ------ | ---------------------------------------------------------------------------------- |
| `-f`   | Subdomain file (required)                                                          |
| `-x`   | Run HTTPX for live host filtering (`run`) or provide an existing HTTPX output file |
| `-v`   | Enable verbose logging                                                             |
| `-d`   | Custom scan results directory (default: `~/scan_results`)                          |
| `-l`   | Log file name (optional, stored inside scan directory)                             |

### Examples

1. **Basic scan**:

```bash
./auto_nuclei.sh -f subs.txt
```

2. **Scan with HTTPX live filtering**:

```bash
./auto_nuclei.sh -f subs.txt -x run
```

3. **Verbose scan with custom folder and log**:

```bash
./auto_nuclei.sh -f subs.txt -v -d ~/my_scans -l scan.log
```

---

## How It Works

1. **System Detection**: Detects CPU cores, available RAM, disk space, and GPU (if available). If any command is missing, safe default values are used.
2. **Adaptive Tuning**:

   * Chunk size is calculated based on available RAM.
   * Concurrency and rate limits are based on CPU threads.
3. **HTTPX Integration**: Optional step to filter live hosts based on HTTP status codes.
4. **Target Chunking**: Splits the subdomain list into chunks for parallel scanning.
5. **Nuclei Scan**:

   * Each chunk is scanned with Nuclei.
   * Retries are attempted if a chunk fails.
6. **Results Compilation**: All chunk outputs are merged, deduplicated, and stored in the scan folder.

---

## Output

* `scan_results/nuclei_results.txt` → Final merged Nuclei results
* `scan_results/nuclei_chunk_*` → Individual chunk outputs
* `scan_results/live.txt` → HTTPX output (if used)
* `scan_results/live_filtered.txt` → Filtered live hosts (if HTTPX used)

---

## Adaptive Defaults

| Resource    | Low                  | Medium               | High                 |
| ----------- | -------------------- | -------------------- | -------------------- |
| RAM         | <2GB                 | 2-8GB                | >8GB                 |
| Chunk size  | 300                  | 600-1200             | 2000                 |
| Concurrency | CPU threads (max 12) | CPU threads (max 12) | CPU threads (max 12) |

---

## Notes

* Ensure subdomain file is clean (one domain per line).
* Safe to run on minimal systems; uses fallback defaults if commands are missing.
* GPU detection is informational; future versions may leverage GPU for concurrency.

---

## License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

## Contribution

Contributions are welcome! Open an issue, submit improvements, or create a pull request.

---
