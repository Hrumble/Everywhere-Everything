**Domain Name Server**
[RFC 1034](https://datatracker.ietf.org/doc/html/rfc1034)

>[!quote] The primary goal is a consistent name space which will be used for referring to resources.  In order to avoid the problems caused by ad hoc encodings, names should not be required to contain network identifiers, addresses, routes, or similar information as part of the name.

Originally, name to address mappings (e.g. `www.example.com` to `31.324.21.43`) were maintained by the **Network Information Center** (NIC) in a single `HOST.txt` file which was sent over [[FTP]] to all hosts. (*you can find this host file (local) still inside your linux `/etc/host.txt`*)
This was ok at the start, but quickly grew inefficient as the number of websites and hosts grew larger. 

**DNS** was invented to fix that.

>[!warning] The terms "domain" or "domain name" are used in many contexts beyond the DNS described here.  
>Very often, the term domain name is used to refer to a name with structure indicated by dots, but no relation to the DNS. This is particularly true in mail addressing.

>[!info] The DNS directory isn't located in one physical place or even one corner of the vast Internet. It's distributed **all over the world** and stored on many different servers that communicate with one another to regularly provide updates, information, and redundancies.

Just like the `/etc/host.txt` file, a DNS server is simply a server hosting a text file mapping name to addresses. 
A **DNS Server** is a server that hosts a multitude of text files, each titled after the domain name it stores information for. For instance, there could be a [example.com](example.com) file, with inside all of its [[DNS#DNS Records|DNS Records]].
This is a **namespace**, which is a set of names used to refer to objects which all have **unique** names.
*that's why you can't have two websites with the same domain name, otherwise your packet won't know which [[IP]] address to go to*.

# Registrar

>[!important] A domain name itself comprises two elements: before and after “the dot”. The part to the right of the dot, such as “com”, “net”, “org” and so on, is known as a “top-level domain” or TLD. One company in each case (called a registry), is in charge of all domains ending with that particular TLD and has access to a full list of domains directly under that name, as well as the IP addresses with which those names are associated.

When using [[WHOIS]], the client requests the information from the **registrar**, the **registrar** is a company that ensures certain domain names are available, keeps recorded information on each domain name. **However**, it **does not** handle **Domain Name Resolution**, this is handled by a **DNS** server such as **Cloudfare's** `1.1.1.1` or **Google's** `8.8.8.8`.

>[!warning] `1.1.1.1` or `8.8.8.8` does not refer to a single physical device on the internet.
>*this would be insane and terribly slow*
>
>It refers to an **Anycast** network, which is a networking technique consisting of having multiple hosts with the same address. 
>With the help of the [BGP](https://www.cloudflare.com/en-gb/learning/security/glossary/what-is-bgp/), the client accesses the server which is the fastest to him.

# DNS Records
For each domain, DNS records are created, all having their own uses. Some common ones include **A**, **AAAA**, **CNAME**, **MX**.
Let's look at what they're used for.

## A Record
An **A** Record is used to point a domain to a single [[IP#Version|IPv4]] address.
You could have multiple **A** records, each pointing a different subdomain to a different IP, or the same for that matter.

Each **A** Record has a name field, which indicates which part of the domain it should act on. Common values are
- `@` To take effect on the **SLD** (Second Level Domain), in `www.example.com` this is `example.com`.
- `*` Which indicates a wildcard that takes effect on **any** subdomain.
- `www` or `ns1` or `whateverfuckyou` which specifies a particular subdomain, anything can go here.

Here's what the **A** Records of [example.com](example.com) would look like in their respective file.
```sh
@ IN A 192.168.1.10
www IN A 192.168.1.10
secretsubdomain IN A 192.168.1.20
```
This means that `example.com`, and `www.example.com` route to `192.168.1.10`. and `secretsubdomain.example.com` routes to `192.168.1.20`

## AAAA Record
The **AAAA** is the same as the [[DNS#A Record|A Record]] except for the fact that it points to [[IP#Version|IPv6]] address.

## CNAME Record
The **CNAME** Record a.k.a **Canonical Name Record** is used to point a **subdomain** (bad practice to be used on root domains) to another domain name. This is typically used to avoid redundancy and ensure that **even if your server's IP change, you don't have to change every other record**.

A CNAME record is mainly used on domain names with subdomains that each point to the same IP Address.

Here's what a CNAME Record would look like in example.com:
```sh
@ IN A 192.168.1.10
www IN CNAME example.com
secretsubdomain IN A 192.168.1.20
```
The CNAME now points to the root domain, so it will always be redirected to the address specified in the **A Record**.

