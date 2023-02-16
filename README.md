# netcat

Netcat is the Swiss knife of networking, so why not create it
in Python? Such a usefool tool would be quite an asset if an
attacker managed to find a way in. With it, one can read and 
write data across the network, meaning it can be used to
execute remote commands, pass files back and forth, or
even open up an interractive shell. Let's get started:

Libraries used include argparse, socket, shlex, subprocess,
sys, textwrap and threading.

The argparse module form the standard library is used to
create a command line interface. Example usage cases are
provided in the source code (netcat.py):

- The `-c` argument sets up an interactive shell;
- The `-e` argument executes one specific command;
- The `-l` argument indicates that a listener should be set up;
- The `-p` argument specifies the port on which to communicate;
- The `-t` argument specifies the target IP;
- The `-u` argument specifies the name of a file to upload.

The `-c`, `-e`, and `-u` arguments imply the `-l` argument, 
because those arguments apply to only the listener side of the 
communication. The sender side makes the connection to the
listener, and so it needs only the `-t` and `-p` arguments to 
define the target listener.

If Netcat is set up as a listener, a NetCat object is invoked
with an empty buffer string. In other words, there is no
need to send anything (yet). Otherwise, we send the buffer
containing the user input (using STDIN).

NetCat's *handle* method executes the task corresponding to the 
command received. The task can be either a command, file or
a request to open an interractive shell.

**Notice** that the shell scans for a newline character to
determine when to process a command. This implementation makes
it netcat friendly. That is, you can use this program on the 
listener side and use netcat itself on the sender side.