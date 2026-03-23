# TCP File Transfer — Client/Server in C

A simple TCP-based file transfer application written in C using POSIX sockets.
The server handles multiple clients concurrently using `fork()`.

## Features
- Multi-client support via process forking
- File transfer with size-header protocol
- Error handling for missing files and bad connections
- Command-line driven — no hardcoded IPs or ports

## Project Structure
```
.
├── Server.c
├── Client.c
├── Makefile
└── README.md
```

## Build
```bash
make
```

Or manually:
```bash
gcc Server.c -o server
gcc Client.c -o client
```

## Usage

**Start the server** (pick any available port):
```bash
./server 9000
```

**Run the client** (in another terminal):
```bash
./client 127.0.0.1 9000 Demo.txt Downloaded.txt
```

| Argument | Description |
|----------|-------------|
| `127.0.0.1` | Server IP address |
| `9000` | Server port |
| `Demo.txt` | File to request from server |
| `Downloaded.txt` | Name to save it as on client side |

## Protocol
```
Client  ──►  "Demo.txt\n"          (filename request)
Server  ──►  "OK 1700\n"           (header: status + file size)
Server  ──►  [raw file bytes]      (actual file data)
```

If the file doesn't exist, server responds with `ERR` instead of `OK`.

## Requirements
- Linux / macOS
- GCC compiler

## Notes
- Server runs indefinitely until manually stopped (`Ctrl+C`)
- Each client is handled in a separate child process
- Port numbers below 1024 require root privileges
- 
