<p align="center">
  <img src="https://github.com/user-attachments/assets/6a168d5f-5e36-4e02-b150-e6269698ebf6" 
       alt="FileFlow"
       width="60%" />
</p>

# CIDR to IP List

A lightweight Python utility that expands one or more CIDR (Classless Inter-Domain Routing) notations into a flat list of individual IPv4 addresses and writes them to a text file.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Input Format](#input-format)
- [Output Format](#output-format)
- [Configuration](#configuration)
- [Example](#example)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

When working with firewalls, access control lists, security tools, or network scanning, you often need to enumerate every individual IP address that belongs to a subnet. This tool automates that process: supply a list of CIDR blocks and it produces a complete, line-separated list of every IP address within those ranges.

---

## Features

- Converts any number of IPv4 CIDR notations to individual IP addresses in one run.
- Reads input from a plain-text file — one CIDR per line.
- Appends results to `results.txt` by default, preserving previous runs.
- Optionally overwrites the output file on each run (see [Configuration](#configuration)).
- Zero external dependencies — uses only Python's standard library (`ipaddress`, `os`).

---

## Requirements

- Python 3.3 or later (the `ipaddress` module is part of the standard library since Python 3.3).

No third-party packages need to be installed.

---

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/ali-rajabpour/CIDR-to-IP-List.git
   cd CIDR-to-IP-List
   ```

2. No further installation steps are required.

---

## Usage

1. Create a file named `cidrs.txt` in the same directory as `cidr_to_ip.py`.
2. Add one CIDR notation per line (see [Input Format](#input-format)).
3. Run the script:

   ```bash
   python cidr_to_ip.py
   ```

4. The expanded IP addresses are written to `results.txt` in the same directory.

---

## Input Format

`cidrs.txt` must contain one valid IPv4 CIDR block per line. Empty lines are not supported and should be avoided.

```
192.168.1.0/24
10.0.0.0/30
172.16.0.0/28
```

---

## Output Format

`results.txt` will contain one IP address per line, in ascending order, for each CIDR block listed in the input file. Results from multiple CIDR blocks are appended sequentially.

```
192.168.1.0
192.168.1.1
192.168.1.2
...
192.168.1.255
10.0.0.0
10.0.0.1
10.0.0.2
10.0.0.3
```

---

## Configuration

### Overwrite vs. Append

By default the script opens `results.txt` in **append** mode (`'a'`), meaning each run adds new IP addresses after any previously written content.

To **overwrite** the file on every run (starting fresh each time), open `cidr_to_ip.py` and change the file mode on this line:

```python
# Default (append):
with open(filepath, 'a') as f:

# Change to overwrite:
with open(filepath, 'w') as f:
```

---

## Example

Given the following `cidrs.txt`:

```
203.0.113.0/30
```

After running `python cidr_to_ip.py`, `results.txt` will contain:

```
203.0.113.0
203.0.113.1
203.0.113.2
203.0.113.3
```

The terminal will also print a confirmation:

```
IP addresses from ['203.0.113.0/30'] saved to /path/to/results.txt
```

---

## Project Structure

```
CIDR-to-IP-List/
├── cidr_to_ip.py   # Main script
├── cidrs.txt       # Your input file (create this before running)
├── results.txt     # Generated output file (created automatically)
└── README.md
```

---

## How It Works

1. **`read_cidr_file(cidr_file)`** — Opens `cidrs.txt` and returns a list of CIDR strings (one per line).
2. **`cidr_to_ips(cidr)`** — Uses `ipaddress.IPv4Network` to iterate over every host address in the given CIDR block and returns them as a list of strings.
3. **`save_to_file(ips, filepath)`** — Appends each IP address followed by a newline character to the output file.
4. The main block iterates over all CIDRs, expands each one, and writes the results to `results.txt` located in the same directory as the script.

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature-name`.
3. Make your changes and commit: `git commit -m "Add your feature"`.
4. Push to your fork: `git push origin feature/your-feature-name`.
5. Open a Pull Request.

Please ensure your code follows the existing style and includes a clear description of the changes.

---

## License

This project is open source. See the repository for details.

---

*By [Ali Rajabpour-Sanati](https://github.com/ali-rajabpour)*
