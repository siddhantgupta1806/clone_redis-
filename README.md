# Redis Clone

A lightweight Redis-like server implemented in C++ that supports basic key-value operations, expiration, and sorted sets.

## Overview

This project is a simplified in-memory server inspired by Redis. It demonstrates core concepts such as:

- In-memory key-value storage
- A custom binary protocol for client-server communication
- Support for string values, expiration, and sorted sets
- Command processing through a structured command system
- Non-blocking I/O using an event loop architecture

## Tech Stack

- Language: C++17
- Build tool: Makefile with g++
- Networking: TCP sockets and non-blocking I/O
- Data structures: hash tables, AVL trees, and TTL-based management
- Protocol: custom binary request/response format

## Prerequisites

- A C++17-compatible compiler such as g++
- A Linux or Unix-like environment

## Build

From the project root, run:

```bash
make
```

This builds both the server binary and the client binary.

## Run

### Start the server

```bash
./redis-clone
```

The server listens on 0.0.0.0:8080 by default.

### Start the client

```bash
./client
```

## Supported Commands

### Basic commands

- `SET key value` — store a value
- `GET key` — retrieve a value
- `DEL key` — delete a key
- `KEYS` — list all keys
- `PTTL key` — get the remaining time-to-live in milliseconds
- `PEXPIRE key milliseconds` — set a key's expiration time

### Sorted set commands

- `ZADD key score member` — add a member to a sorted set
- `ZREM key member` — remove a member
- `ZSCORE key member` — get the score of a member
- `ZQUERY key min max` — query members within a score range

### Example usage

```text
SET mykey hello
GET mykey
PTTL mykey
PEXPIRE mykey 1500
PTTL mykey
DEL mykey

ZADD ranks 10 alice
ZADD ranks 5 bob
ZSCORE ranks alice
ZQUERY ranks 0 10
```

## Protocol Details

### Request format

```text
msglen (4 bytes) | nargs (4 bytes) | arglen1 (4 bytes) | arg1 | ... | arglenN (4 bytes) | argN
```

### Response format

```text
msglen (4 bytes) | tag (1 byte) | payload
```

The response tag identifies the type of data returned, such as NIL, ERROR, STRING, INTEGER, DOUBLE, or ARRAY.

## Architecture

The project is organized into several main components:

- **Server** — starts the application and manages runtime flow
- **EventLoop** — handles non-blocking I/O events
- **ConnectionManager** — manages active client connections
- **RequestProcessor** — parses and routes incoming requests
- **ProtocolHelper** — encodes and decodes the custom binary protocol
- **Command system** — executes supported commands through command objects
- **Database layer** — stores data in memory using hash tables and AVL trees

## Input and Output

- Input is provided through the client program in [client.cpp](client.cpp)
- Incoming requests are processed in [src/server/RequestProcessor.cpp](src/server/RequestProcessor.cpp)
- Network communication is handled in [src/network/Connection.cpp](src/network/Connection.cpp)
- The binary protocol is implemented in [src/protocol/ProtocolHelper.cpp](src/protocol/ProtocolHelper.cpp)

## Attribution

This project is based on concepts and structures inspired by the book "Build Your Own Redis with C/C++" by James Smith.

