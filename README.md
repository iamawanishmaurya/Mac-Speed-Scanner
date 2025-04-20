# MAC Speed Scanner

## Overview
MAC Speed Scanner is a Linux utility that tests internet performance across different MAC addresses on your network interface. It helps identify which MAC address provides the best internet speeds, which can be useful for optimizing network performance or bypassing certain network restrictions.

## Features
- Automatically scans local network for MAC addresses or reads from a file
- Changes network interface's MAC address and runs speed tests on each one
- Measures download speed, upload speed, and ping for each MAC address
- Sorts and ranks MAC addresses by download speed performance
- Supports major Linux distributions (Ubuntu, Debian, Fedora, Arch, etc.)
- Automatically installs necessary dependencies

## Prerequisites
- Linux operating system
- Root/sudo privileges
- Working internet connection
- Python 3.x

## Installation

1. Download the script:
```bash
wget https://raw.githubusercontent.com/yourusername/mac-speed-scanner/main/mac_speed_scanner.py
```

2. Make the script executable:
```bash
chmod +x mac_speed_scanner.py
```

## Configuration

Before running the script, you may want to modify the following configuration variables at the top of the script:

```python
# Configuration
INTERFACE = "wlan0"  # Change to your WiFi interface
MAC_ADDRESSES_FILE = "mac_address.txt"  # File to read/write MAC addresses
RESULTS_FILE = "result.txt"  # File to write results
USE_LOCAL_SCAN = True  # Set to True to use arp-scan instead of one.txt
```

- `INTERFACE`: Set this to your network interface (e.g., "wlan0", "eth0", "enp3s0")
  - To find your interface name, run: `ip a` or `ifconfig`
- `MAC_ADDRESSES_FILE`: The file where MAC addresses are read from or written to
- `RESULTS_FILE`: Where the speed test results will be saved
- `USE_LOCAL_SCAN`: Whether to scan the local network for MAC addresses (True) or read from file (False)

## Usage

### Basic Usage

Run the script with root privileges:

```bash
sudo python3 mac_speed_scanner.py
```

### Using Your Own List of MAC Addresses

1. Create a file named `mac_address.txt` with one MAC address per line:
```
AA:BB:CC:DD:EE:FF
11:22:33:44:55:66
```

2. Set `USE_LOCAL_SCAN = False` in the script or run:
```bash
sudo python3 mac_speed_scanner.py --use-file
```

### Process Flow

When you run the script:

1. It checks for root privileges
2. If `USE_LOCAL_SCAN` is True:
   - Installs `arp-scan` if not present
   - Scans the local network for MAC addresses
   - Saves them to `one.txt`
3. If `USE_LOCAL_SCAN` is False or the scan finds no addresses:
   - Reads MAC addresses from `mac_address.txt`
4. For each MAC address:
   - Changes your network interface's MAC address
   - Waits for the connection to stabilize
   - Installs `speedtest-cli` if not present
   - Runs a speed test
   - Records download/upload speeds and ping
5. Sorts the results by download speed
6. Saves detailed results to `result.txt`

## Understanding the Results

The script creates a `result.txt` file with results sorted by download speed (highest first). The file includes:

- Test timestamp
- Total MAC addresses tested
- Number of successful and failed tests
- A table with columns:
  - MAC Address
  - Download speed (Mbps)
  - Upload speed (Mbps)
  - Ping (ms)
  - Rank

Example:
```
MAC Address Test Results - 2025-04-20 15:30:45
Total MAC addresses tested: 10
Successful tests: 8
Failed tests: 2
---------------------------------------------------------------------------
MAC Address         Download (Mbps) Upload (Mbps)   Ping (ms)  Rank      
---------------------------------------------------------------------------
AA:BB:CC:DD:EE:FF   95.42           45.67           12.3       1         
11:22:33:44:55:66   82.31           40.12           15.7       2         
...
```

## Troubleshooting

### "None" Values in Speed Test Results

If you see "None" or "Error" for speed test results:

1. **Check Internet Connection**: Ensure your internet is working properly
2. **speedtest-cli Issues**: Try running `speedtest-cli --simple` manually
3. **Network Restrictions**: Some networks block speed testing services
4. **Interface Problems**: Verify the interface name (`ip a`) and update the `INTERFACE` variable
5. **Wait Longer**: Increase the wait time after changing MAC address (change the `time.sleep(10)` value)

### MAC Address Changes Not Working

If MAC address changes fail:

1. **Hardware Limitations**: Some network interfaces don't support MAC address changes
2. **Driver Issues**: Update your network drivers
3. **Interface Name**: Double-check your interface name with `ip a`

### Permission Issues

If you get permission errors:

1. **Run as Root**: Always use `sudo` to run the script
2. **File Permissions**: Make sure the script is executable (`chmod +x mac_speed_scanner.py`)

## Advanced Usage

### Testing Specific MAC Addresses

To test only specific MAC addresses:
1. Create `mac_address.txt` with your desired MAC addresses
2. Set `USE_LOCAL_SCAN = False` in the script

### Changing Test Parameters

- To change the wait time after changing MAC address, modify the `time.sleep(10)` line
- To use a different speed test service, modify the `run_speedtest()` function

## License

This software is provided "as is" without warranty of any kind. Use at your own risk.

## Contributing

Feel free to submit pull requests or report issues on the GitHub repository.
