**File Transfer [[What is a protocol|Protocol]]**
[RFC 959](https://datatracker.ietf.org/doc/html/rfc959)


>[!quote] The Objectives of **FTP** Are
> 1) to promote sharing of files (computer programs and/or data)
>2) to encourage indirect or implicit (via programs) use of remote computers 
>3) to shield a user from variations in file storage systems among hosts, and 
>4) to transfer data reliably and efficiently.

Though usable directly by a user at a terminal, it's designed mainly for programs

**FTP** is a simple and efficient way to transfer files between two hosts, it does not have a particular header type, for a request it just contains a `Request Command` and `Request arg`, and for a Reply it contains a `Response code`, and `Response Arg`. 
Basically, the **FTP** consists of sending [[#Commands]] following the [[Telnet]] protocol (which means clear text, terminal to terminal) .


# Commands

The following are the FTP command list, I will describe those that are the most common *(you will never use `ACCT`🤦‍♂️)*

>[!info] The `<SP>` refers to a **space character**, `<CRLF>` means **new line** (*`CR` moves cursor to beginning of line and `LF` moves it one line under*)

```
USER <SP> <username> <CRLF> 
PASS <SP> <password> <CRLF> 
ACCT <SP> <account-information> <CRLF> 
```
```
CWD <SP> <pathname> <CRLF>
```
**Change Working Directory**, similar to `cd`, allows the user to change his working directory without having to log back in on another dir.
```
CDUP <CRLF>
```
**Change To Parent Directory**, as the name implies.
```
SMNT <SP> <pathname> <CRLF> 
QUIT <CRLF> 
REIN <CRLF> 
PORT <SP> <host-port> <CRLF>
```
**Data Port**, specification for the data port to be used in data connection.  There are defaults for both the user and server data ports, and under normal circumstances this command and its reply are not needed
```
PASV <CRLF>
```
**Passive**, This command requests the server-DTP to "listen" on a data port (which is not its default data port) and to wait for a connection rather than initiate one upon receipt of a transfer command.  The response to this command includes the host and port address this server is listening on.
```
TYPE <SP> <type-code> <CRLF> 
STRU <SP> <structure-code> <CRLF> 
MODE <SP> <mode-code> <CRLF>
```
**Transfer Mode**, the argument is a single [[Telnet]] character code specifying the data transfer modes described in the Section on Transmission Modes.

The following codes are assigned for transfer modes:
   - S - Stream
   - B - Block
   - C - Compressed
The default transfer mode is Stream.

```
RETR <SP> <pathname> <CRLF> 
```
**Retrieve**, downloads a copy of the specified file.
```
STOR <SP> <pathname> <CRLF>
```
**Store**, stores a specific file on the server. If a file already exists at the given path name, then it's contents will be replaced, otherwise the file will be created.
```
STOU <CRLF> 
```
**Store Unique**, stores a file on the server with a name unique to that server, The 250 Transfer Started response must include the name generated.
```
APPE <SP> <pathname> <CRLF>
```
**Append**, similar to store, except if the file already exists, instead of it being overwritten, the data will be appended to it.
```
ALLO <SP> <decimal-integer> [<SP> R <SP> <decimal-integer>] <CRLF>
```
**Allocate**, allocates a space on the server for future use.
```
REST <SP> <marker> <CRLF> 
RNFR <SP> <pathname> <CRLF>
```
**Rename From**, specifies a file to be renamed, **MUST** be followed by **Renamed To**
```
RNTO <SP> <pathname> <CRLF>
```
**Rename To**, specifies the new pathname of the file specified in the preceding Rename From.
```
ABOR <CRLF> 
DELE <SP> <pathname> <CRLF>
```
**Delete**, deletes the specified file.
```
RMD <SP> <pathname> <CRLF>
```
**Remove Directory**, deletes a directory along with it's contents.
```
MKD <SP> <pathname> <CRLF>
```
**Make Directory**, similar to `mkdir`, creates a directory at the specified path name.
```
PWD <CRLF> 
```
**Print Working directory**... self explanatory?
```
LIST [<SP> <pathname>] <CRLF> 
```
**List**, equivalent of `ls`, except you can specify a file to get info on that particular file too.
```
NLST [<SP> <pathname>] <CRLF> 
```
**Name List**, will return a list of file names **only**.
```
SITE <SP> <string> <CRLF> 
SYST <CRLF> STAT [<SP> <pathname>] <CRLF> 
HELP [<SP> <string>] <CRLF> 
NOOP <CRLF>
```
**Noop**, a cute little ping, server will send back **OK**, check if your server is **ok** from time to time 🥰

>[!warning] These are not to be mistaken with terminal commands that you pass through your [[Terminal Emulators]], these are **official FTP commands that are used to communicate on the protocol level**
>Of course, when using a FTP compliant application, you can enter most of these commands, but this is for ease of understanding, these commands are the ones that are passed over the network by the **user PI** (*Protocol Interpreter*) for the **server PI** to execute the specific task.

