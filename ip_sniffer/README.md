# IP Sniffer

A simple multi-threaded TCP port scanner written in Rust.

## Description

This command-line tool scans a target IP address (both IPv4 and IPv6 are supported) to discover which of its TCP ports are open. It leverages multithreading to perform the scan concurrently, which can significantly speed up the discovery process.

## How It Works

The program parses command-line arguments to get the target IP address and the desired number of threads.

1.  It spawns a user-specified number of worker threads (or a default of 4 if not specified).
2.  The port range (1-65535) is divided among the threads. Each thread starts at a different port and increments by the total number of threads. For example, with 100 threads, thread `0` would check ports 1, 101, 201, etc., while thread `1` checks ports 2, 102, 202, and so on.
3.  Each thread attempts to establish a `TcpStream` connection to its assigned ports on the target IP.
4.  If a connection is successful, the port is considered "open," and the port number is sent back to the main thread via a shared channel.
5.  The main thread collects all the open port numbers from the channel, waits for all threads to complete, sorts the results, and prints the list of open ports to the console.

## Usage

You can run the program from your terminal using `cargo run`.

### Basic Scan

To scan an IP address with the default number of threads (4):

```sh
cargo run -- <IP_ADDRESS>
```

**Example:**
```sh
cargo run -- 127.0.0.1
```

### Specifying Thread Count

Use the `-j` flag to specify the number of threads you want to use for the scan.

```sh
cargo run -- -j <NUMBER_OF_THREADS> <IP_ADDRESS>
```

**Example (using 100 threads):**
```sh
cargo run -- -j 100 127.0.0.1
```

### Help

To display the help message with usage instructions, use the `-h` or `-help` flag.

```sh
cargo run -- -h
```