# socketserverbroadcast

A Python socket server library with broadcast capabilities, providing generic socket server classes for building network applications.

## Features

- Support for multiple address families (AF_INET, AF_INET6, AF_UNIX)
- Support for multiple socket types (SOCK_STREAM, SOCK_DGRAM)
- Multiple request handling modes:
  - Synchronous (one request at a time)
  - Forking (each request handled by a new process)
  - Threading (each request handled by a new thread)
- TCP and UDP server implementations
- Unix domain socket support
- Built-in server mix-in classes for threading and forking

## Installation

### From GitHub

You can install this package directly from GitHub using pip:

```bash
pip install git+https://github.com/yourusername/socketserverbroadcast.git
```

### From source

Clone the repository and install:

```bash
git clone https://github.com/yourusername/socketserverbroadcast.git
cd socketserverbroadcast
pip install .
```

### Development installation

For development, install with dev dependencies:

```bash
pip install -e ".[dev]"
```

## Usage

Here's a basic example of creating a TCP server:

```python
import socketserverbroadcast

class MyTCPHandler(socketserverbroadcast.BaseRequestHandler):
    def handle(self):
        # self.request is the TCP socket connected to the client
        self.data = self.request.recv(1024).strip()
        print(f"Received from {self.client_address[0]}: {self.data}")
        # Echo back to client
        self.request.sendall(self.data.upper())

if __name__ == "__main__":
    HOST, PORT = "localhost", 9999
    
    # Create the server
    with socketserverbroadcast.TCPServer((HOST, PORT), MyTCPHandler) as server:
        # Activate the server; this will keep running until interrupted
        server.serve_forever()
```

### Threading Server Example

```python
import socketserverbroadcast

class ThreadedTCPServer(socketserverbroadcast.ThreadingMixIn, 
                        socketserverbroadcast.TCPServer):
    pass

if __name__ == "__main__":
    HOST, PORT = "localhost", 9999
    server = ThreadedTCPServer((HOST, PORT), MyTCPHandler)
    server.serve_forever()
```

## Server Classes

- `BaseServer` - Base class for all server types
- `TCPServer` - TCP/IP server
- `UDPServer` - UDP/IP server
- `UnixStreamServer` - Unix domain socket stream server
- `UnixDatagramServer` - Unix domain socket datagram server
- `ThreadingMixIn` - Mix-in class for threading support
- `ForkingMixIn` - Mix-in class for forking support (Unix only)

## Request Handler Classes

- `BaseRequestHandler` - Base class for request handlers
- `StreamRequestHandler` - Handler for stream-based protocols (TCP)
- `DatagramRequestHandler` - Handler for datagram protocols (UDP)

## Requirements

- Python >= 3.7

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Changelog

### Version 0.5
- Initial packaged release
