# Systems Programming (Rutgers, Fall 2018)

C projects from Rutgers CS 214, Systems Programming. They cover file I/O, sorting, processes, directory traversal, sockets and multithreading. Each project builds on the one before it.

| Project | What it does | Key concepts |
|---|---|---|
| [PA0](PA0/) | **CSV sorter.** Reads a CSV from standard input and sorts the rows by a chosen column. | Parsing, dynamic memory, merge sort |
| [PA1](PA1/) | Refined version of the CSV sorter that detects whether a column holds numbers or text and sorts it accordingly. | Linked-list merge sort, type detection |
| [PA2](PA2/) | **Recursive directory sorter.** Walks a directory tree and sorts every valid CSV it finds, forking a process for each file and subdirectory. | `fork`, `wait`, directory traversal, process IDs |
| [PA3](PA3/) | **Multithreaded banking server and client.** Clients connect over TCP to open accounts and deposit or withdraw money. The server prints a status report of all accounts every 15 seconds. | Sockets, `pthreads`, semaphores and mutexes, signal handling |

## Usage

**PA0 / PA1: sort a CSV by column**
```bash
gcc -o simpleCSVsorter simpleCSVsorter.c mergesort.c
cat movie_metadata.csv | ./simpleCSVsorter -c movie_title > sorted.csv
```

**PA2: sort every CSV under a directory**
```bash
./scannerCSVsorter -c <column> [-d <input dir>] [-o <output dir>]
```
Sorted files are written as `<name>-sorted-<column>.csv`. Each process records its PID, and the main process prints a summary of all the processes it created.

**PA3: banking server and client**
```bash
make
./bankingServer <port>                    # port 8192–65535
./bankingClient <server-hostname> <port>
```
Client commands:

| Command | Effect |
|---|---|
| `create <name>` | Open a new account |
| `serve <name>` | Start a session on an account |
| `deposit <amount>` / `withdraw <amount>` | Change the balance of the account being served |
| `query` | Show the current balance |
| `end` | End the session on the account |
| `quit` | Disconnect from the server |

The server handles each client on its own thread and locks shared account data so that concurrent sessions stay consistent.
