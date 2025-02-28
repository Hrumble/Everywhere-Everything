**Secure Shell**
[RFC 4252](https://datatracker.ietf.org/doc/html/rfc4252) [RFC 4253](https://datatracker.ietf.org/doc/html/rfc4253) [RFC 4254](https://datatracker.ietf.org/doc/html/rfc4254)

**The SSH protocol has 3 layers of communication thus the 3 RFCs**

>[!info]    The Secure Shell (SSH) is a protocol for secure remote login and other secure network services over an insecure network. \[...] It provides strong encryption, server authentication, and integrity protection.  It may also provide compression.

**SSH** is just a more secure [[Telnet]].

An SSH packet comprises of  
`4Bytes` **Message Length** - The **size** of the packet
`1 Byte` **Padding Size** - will become relevant be patient
`Variable size` **Payload** - The actual data sent over **SSH**
`Variable size` **Padding** - The actual padding whose size was defined earlier, this allows the host computer to know when the payload ends.
`8 Byte` **Checksum** - Makes sure the packet arrived well and in one piece (*which is the friends we made along the way*)

The encryption happens when the packet leaves the client computer, and encrypts the entirety of the **Padding Size**, **Payload**, and **Padding**, it's then decrypted by the host computer on arrival.

# Encryption and Keys

>[!faq] that's great ugly boy but how exactly does this encryption happen, and what makes it so secure that absolutely everyone uses it?

The encryption used here is called **Asymmetric Encryption**. 
**Asymmetric encryption** works by generating two **Keys** for each client that should be able to receive messages. 
The first key is the **Private Key**. It allows the host to decrypt messages that were encrypted following a particular algorithm.
The second key is the **Public Key**. It allows anyone who owns it to encrypt messages decryptable **ONLY** by the **Private key**.

>[!info] Following a mathematical algorithm, the two keys are linked for each host, and **ONLY** **Private Key A** can decrypt messages encrypted by **Public Key A**.

let's imagine **Alice** , and **Bob**. *this is alice btw*
![[ana_armas.jpg|100]]

Alice wants to send an **encrypted message** to her friend **Bob**. However, wants to make **SURE** only bob is able to decrypt it (*it's a cybernude*). To do so, **Alice** requests **Bob** public key, encrypts her *cybernude* with it, then sends it to bob.
**Now**, if anyone where to somehow find that *cybernude* somewhere, they would be able to decrypt it **only** using **Bob's Private Key**, since it was encrypted via it's public key.

>[!warning] it's **IMPOSSIBLE** to derive the private key from the public key. **However**, it is possible to derive the public key from the private key.

## SSH Keys

Now that we understand Asymmetric encryption, let's see how **SSH** implements that to stay safe (*or not :D*). When your **Client SSH** connects for the **first time** to the **Server SSH**, the Server sends it's public key to you, and asks you if you trust this key by sending you it's **SHA256** fingerprint.
```shell
$ ssh root@192.168.0.3

The authenticity of host '192.168.0.3 (192.168.0.3)' can't be established.
ED25519 key fingerprint is SHA256:5UfoU0l3OByUXIwKPIpk66UXvLYNBcC85iNy6oDYb2w.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
*the sha256 fingerprint is used to verify the integrity of an object, it's a non reversible 256 bit **hash** which produces a **unique** hash for any file given to it*

When you enter in `yes`, your SSH client stores that public key in `~/.ssh/known_hosts`.
This makes it so that on your next connection to the **Server SSH**, when the server will once again send you it's public key, your client will compare that key to the one(s) in `~/.ssh/known_hosts`, if it matches, then your client knows that no one is trying to impersonate that previous **Server**. (**MITM**)

On the server side, when authenticating using his public key, the client sends his public key to the server, which then **compares it to his list of authorized public keys inside `~/.ssh/authorized_keys`**, if the key matches one of the entries, then the server allows connection.

>[!info] In this situation, the server has the client's public key, and the client has the server's public key, which allow both of them to communicate back and forth.

>[!info] If an attacker gains access to a legitimate `authorized_keys` public key
>The connection to the server would be allowed. However, the attacker would not be able to decrypt any of the data sent to him, since he does not possess the Private Key associated to that public key.

# Connecting via private key

A client can connect to an **SSH Server** using only a public key, as seen above. However, this would not really work as the client would not be able to decrypt any packets coming his way. 
So when connecting to an ssh server, you can specify a keypair to use:

```shell
$ ssh -i ~/ssh_keys/my_key root@123.123.123.123
```
Here, `my_key` is the **Private Key**, and not the public key, this means that the SSH Client (you)
will automatically check the same directory for the matching public key by file name with the `.pub` extension.
For instance, in this case, the SSH Client will look for a `my_key.pub` inside the `~/ssh_keys/` directory.

if no `-i` is precised, then the client looks in `~/.ssh/` for common key names such as `id_rsa` and `id_rsa.pub`.

# Password Authentication

When no key is specified, and no key is found, then SSH fallsback to password auth.
Password auth works the same way except the pairs of private and public keys are generated for this session and this session only.