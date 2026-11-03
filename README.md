![Banner](banner.png)

# net-ipv6-link-local-scanner
Builds the IPv6 link-local address of one local interface from its MAC address and pings that address. Focused on basic network operations.

## Install
`git clone https://github.com/ShadowGuard-AI/net-ipv6-link-local-scanner`

No third party dependencies: this tool uses only the Python standard library, plus the system `ping6` command.

## Usage
`python3 main.py -i INTERFACE [-t TIMEOUT] [-c COUNT] [-v]`

## Parameters
- `-h`: Show help message and exit
- `-i, --interface`: Network interface to use for scanning. Required.
- `-t, --timeout`: Timeout for ping requests in seconds (default: 1).
- `-c, --count`: Number of ping requests to send (default: 1).
- `-v, --verbose`: Enable verbose output for debugging.

## Verified behavior
- `python3 main.py --help` matches the parameter list above, and `python3 -m py_compile main.py` passes. A missing `-i` is rejected by argparse with exit 2, and an unknown interface prints "Invalid interface".
- A run on interface `lo` reported MAC 00:00:00:00:00:00 and "No live hosts found on the local link."
- Scope correction: despite the old README wording, the tool does not enumerate the link and does not discover other hosts. It generates exactly one address, the EUI-64 link-local address of the interface named with `-i`, and pings that one address. A "host found" line therefore reports this interface, with this interface's MAC, not a neighbour.
- If `ping6` is missing, the run exits 1 from inside the ping step. Other ping failures return False and end with "No live hosts found".
- The MAC read uses a Linux ioctl, so this tool is Linux only.

## License
See LICENSE.
