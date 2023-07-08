## Multi-threaded TCP server

- TCP is the most reliable way for two machines to talk to each other over the network.
- Sockets are the logical entity that wraps the entire communication.
- TCP server is a simple process that runs in a machine that listens to a port.

TCP server implementation in Golang,

- Step 1 (Start listening on the port) 
    - When the process starts, pick a port and start listening.
- Step 2 (Wait for the client to connect)
    - Invoke the `Accept` system call and the server would not proceed 
    until some client connects
- Step 3 (Read the request and send the response)
    - Once the connection is established
        1. Invoke the `Read` system call to read the request (**_Blocking_**)
        2. Invoke the `Write` system call to send the response (**_Blocking_**)
        3. Close the connection
- Step 4 (Do this over and over again)
    - Put the `do` function in an infinite for loop...
        1. Continuously waiting for client to connect.
        2. Reading the req.
        3. writing the resp.
        4. closing the conn.
    - It is sequential execution and handling(Accepting one, processing and then
    accepting another)
- Step 5 (Parallelize the processing)
    - One client connects, forks a thread to process and respond (_let the `main` thread come back to `Accept` as quickly as possible_).
    ```go
    for{
        Accept()
        --> process(conn)
    }
    ```
### Problems
1. Can spin up lot of number of thread which is beyond the system capacity and it is
called thread-overloading.
<hr>

### Improvements
1. Limiting the number of threads.
2. Add thread pool to save on thread creation time.
3. Connection timeout (if client doesn't connect in max time, then close the connection and return back the thread to the pool)
4. TCP backlog queue config (system level)


