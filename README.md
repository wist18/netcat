# netcat

Netcat is the Swiss knife of networking, so why not create it
in Python? Such a useful tool would be quite an asset if an
attacker managed to find a way in. With it, one can read and 
write data across the network, meaning it can be used to
execute remote commands, pass files back and forth, or
even open up an interactive shell. Let's get started:

## Libraries used

Libraries used include `argparse`, `socket`, `shlex`, 
`subprocess`, `sys`, `textwrap` and `threading`.

The *argparse* module form the standard library is used to
create a command line interface. 

## Setting up your dev environment

This project has been done on a Linux environment.

Alternative solutions:
- **Linux** works natively (this code is tested on Ubuntu 18.04)
- **Windows** is supported through *WSL2*
- **MacOS** is not supported

If the solutions above do not work, a Virtual Machine 
(e.g., Virtual Box running Ubuntu) is a safe and viable option.

## Setting up Python

This project runs on Python 3.6 or higher. Run the following 
commands to check and update the python version currently installed:

```
python3
```
```
sudo apt-get upgrade python3
```

## Usage
`usage: netcat.py [-h] [-c] [-e EXECUTE] [-l] [-p PORT] [-t TARGET] [-u UPLOAD]`

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

***Notice*** that the shell scans for a newline character to
determine when to process a command. This implementation makes
it netcat friendly. That is, you can use this program on the 
listener side and use netcat itself on the sender side.

## Tutorial - some examples

In one terminal, run the script with the *--help* argument.
```
python3 netcat.py --help
```

Expected output:
```
usage: netcat.py [-h] [-c] [-e EXECUTE] [-l] [-p PORT] [-t TARGET] [-u UPLOAD]

BHP Net Tool

options:
  -h, --help            show this help message and exit
  -c, --command         command shell
  -e EXECUTE, --execute EXECUTE
                        execute specified command
  -l, --listen          listen
  -p PORT, --port PORT  specified port
  -t TARGET, --target TARGET
                        specified IP
  -u UPLOAD, --upload UPLOAD
                        upload file

Example:
                            netcat.py -t 192.168.1.108 -p 5555 -l -c # command shell
                            netcat.py -t 192.168.1.108 -p 5555 -l -u=mytest.txt # upload to file
                            netcat.py -t 192.168.1.108 -p 5555 -l -e="cat /etc/passwd" # execute command
                            echo 'ABC' | ./netcat.py -t 192.168.1.108 -p 135 # echo text to server port 135
                            netcat.py -t 192.168.1.108 -p 5555 # connect to server
```

On the same terminal you can set up a *listener* using your own IP and port.

```
python3 netcat.py -t 192.168.1.203 -p 5555 -l -c
```

***Important!*** The IP adress 192.168.1.203 might be reserved or already in use. The IP adress only serves
as an example. Running it would most likely result in an OSError. Try something like 0.0.0.0 or 127.0.0.1,
unless you are into cryptic error messages.

Now fire up another terminal on your local machine and run the script in *client mode*. Remember that the
script read from STDIN and will do so until it receives the end-of-file (EOF) marker. To send EOF, press
CTRL-D on your keyboard.

```
python3 netcat.py -t 0.0.0.0 -p 5555
```

While not a super technological challenge, this script is a good foundation for discovering security exploits.

## Bonus application:

You can see that we receive our custom command shell. Because we’re
on a Unix host, we can run local commands and receive output in return,
as if we had logged in via SSH or were on the box locally. We can perform
the same setup on your local machine, but have it execute a single command
using the -e switch:

Run the following command in a terminal to run the script in *listener mode*:
```
python3 netcat.py -t 0.0.0.0 -p 5555 -l -e="cat /etc/passwd"
```

Run the following command in a terminal to run the script in *client mode*:
```
python3 netcat.py -t 0.0.0.0 -p 5555
```
