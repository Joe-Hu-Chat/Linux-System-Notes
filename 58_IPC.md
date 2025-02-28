2024年3月27日



IPC: inter-process communication



signals, exit codes,  Named pipes(FIFOs), shared memory, message queues, semaphores, synchronization primitives(mutexes, condition variables)



# Named pipes

ref: https://community.progress.com/s/article/P57906

### EOF condition

An EOF condition is only triggered on the read side when all write descriptors to the pipe are closed, indicating that no more data will be written to the pipe. At that point, any attempt to read from the pipe will return an EOF, indicating that the end of the available data has been reached. 



### Create a named pipe

```bash
mknod pipe_name p

mkfifo pipe_name
```

### write rule

A writter can write data to the named pipe until the named pipe buffer (FIFO queue) is full. the size of this buffer is determined by the PIPE_BUF system parameter in /usr/include/*. (Default is usually 8192 bytes). If the buffer is full, the writer will block until a reader removes data from it.

A writter will block if no read sides exist. To ensure atomicity, Linux will wait having enough space to write in buffer before writing. Be aware that when the data to write is bigger than the buffer size, no atomicity will be ensured.

### read rule

A reader that has opened the named pipe and issued a read() system call will block until there is data in the buffer to read (written by a writer). If there are multiple read operations in a process, only the first read operation will be blocked for data in pipe buffer.

### buffer retention

Data written to the pipe but not read will remain in the buffer provide that the named pipe is kept open by at least one processes. If the named pipe is closed by all readers and writers, any data not yet read from it is lost and subsequent opens will start with an empty buffer.

The main thing to be aware of is that when the reader is blocked waiting for data and the writer closed the named pipe, the reader is returned an EOF status and **per standard 4GL behavior** closes the named pipe as well, which can result in data loss. A way to work around this would be to write a C shared library to read from the pipe and call that from the 4GL.

When writing to a pipe, the following should be taken into account.

**Unbuffered** writing to a pipe means only one character is written at a time. This is slow when compared to buffered writes, and some applications may not be able to handle the unbuffered data properly.

However, if a **buffered** write is used, the buffer is not make available to the reader until the buffer is flushed. This flushing occurs when more data is written to the buffer than the maximum buffer size (BUFSIZ set in stdio.h), or **when the pipe is closed by the writer**. In certain scenarios, this causes issues as some readers will not always wait for the last buffer of data segment to be flushed if it's not flushed immediately. 

### read vs cat

`read data < pipefile` vs `data=$(cat pipefile)`

The `bash`  built-in `read` command is designed to read a line at time, then will not wait for the last buffer of data segment to be flushed, and treat this as an EOF condition. The closing of read side will cause data in pipe losing. 

The `cat` command, on the contrary will wait all the data in pipe buffer to be flushed.

> [!TIP]
>
> Need to study details of the way how `read` and `cat` commands interact with **named pipes**.



