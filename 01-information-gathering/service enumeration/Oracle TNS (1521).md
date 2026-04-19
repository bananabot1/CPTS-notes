**Overview:**
The `Oracle Transparent Network Substrate` (`TNS`) server is a communication protocol that facilitates communication between Oracle databases and applications over networks. TNS supports various networking protocols between Oracle databases and client applications, such as `IPX/SPX` and `TCP/IP` protocol stacks. As a result, it has become a preferred solution for managing large, complex databases in the healthcare, finance, and retail industries. In addition, its built-in encryption mechanism ensures the security of data transmitted, making it an ideal solution for enterprise environments where data security is paramount.
it enables encryption between client and server communication through an additional layer of security over the TCP/IP protocol layer. This feature helps secure the database architecture from unauthorized access or attacks that attempt to compromise the data on the network traffic. Besides, it provides advanced tools and capabilities for database administrators and developers since it offers comprehensive performance monitoring and analysis tools, error reporting and logging capabilities, workload management, and fault tolerance through database services.

The default configuration of the Oracle TNS server varies depending on the version and edition of Oracle software installed. However, some common settings are usually configured by default in Oracle TNS. By default, the listener listens for incoming connections on the `TCP/1521` port. 
The default configuration of the TNS listener includes a few basic security features. For example, the listener will only accept connections from authorized hosts and perform basic authentication using a combination of hostnames, IP addresses, and usernames and passwords. Additionally, the listener will use Oracle Net Services to encrypt the communication between the client and the server. The configuration files for Oracle TNS are called `tnsnames.ora` and `listener.ora` and are typically located in the `$ORACLE_HOME/network/admin` directory. The plain text file contains configuration information for Oracle database instances and other network services that use the TNS protocol.

Oracle TNS is often used with other Oracle services like Oracle DBSNMP, Oracle Databases, Oracle Application Server, Oracle Enterprise Manager, Oracle Fusion Middleware, web servers, and many more. There have been made many changes for the default installation of Oracle services. For example, Oracle 9 has a default password, `CHANGE_ON_INSTALL`, whereas Oracle 10 has no default password set. The Oracle DBSNMP service also uses a default password, `dbsnmp` that we should remember when we come across this one. Another example would be that many organizations still use the `finger` service together with Oracle, which can put Oracle's service at risk and make it vulnerable when we have the required knowledge of a home directory.

Each database or service has a unique entry in the [tnsnames.ora](https://docs.oracle.com/cd/E11882_01/network.112/e10835/tnsnames.htm#NETRF007) file, containing the necessary information for clients to connect to the service. The entry consists of a name for the service, the network location of
the service, and the database or service name that clients should use when connecting to the service.

On the other hand, the `listener.ora` file is a server-side configuration file that defines the listener process's properties and parameters, which is responsible for receiving incoming client requests and forwarding them to the appropriate Oracle database instance.

In short, the client-side Oracle Net Services software uses the `tnsnames.ora` file to resolve service names to network addresses, while the listener process uses the `listener.ora` file to determine the services it should listen to and the behavior of the listener.

##  setting up
```
sudo apt-get update
sudo apt-get install -y build-essential python3-dev libaio1
cd ~
wget https://files.pythonhosted.org/packages/source/c/cx_Oracle/cx_Oracle-8.3.0.tar.gz
tar xzf cx_Oracle-8.3.0.tar.gz
cd cx_Oracle-8.3.0
python3 setup.py build
sudo python3 setup.py install
cd ~
git clone https://github.com/quentinhardy/odat.git
cd odat/
pip install python-libnmap
git submodule init
git submodule update
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor passlib python-libnmap
sudo apt-get install build-essential libgmp-dev -y
pip3 install pycryptodome
```
Before we can enumerate the TNS listener and interact with it, we need to download a few packages and tools for our `Pwnbox` instance

```
./odat.py -h
```
determine if the installation was succesful
Oracle Database Attacking Tool (`ODAT`) is an open-source penetration testing tool written in Python and designed to enumerate and exploit vulnerabilities in Oracle databases. It can be used to identify and exploit various security flaws in Oracle databases, including SQL injection, remote code execution, and privilege escalation.

## enumeration
```
sudo nmap -p1521 -sV 10.129.204.235 --open
```
We can see that the port is open, and the service is running. In Oracle RDBMS, a System Identifier (`SID`) is a unique name that identifies a particular database instance. It can have multiple instances, each with its own System ID

```
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```
bruteforce sids

```
./odat.py all -s 10.129.204.235
```
`odat.py` tool to perform a variety of scans to enumerate and gather information about the Oracle database services and its components. Those scans can retrieve database names, versions, running processes, user accounts, vulnerabilities, misconfigurations, etc.

## Database enumeration

```
sqlplus scott/tiger@10.129.204.235/XE
```
we found valid credentials for the user `scott` and his password `tiger`. After that, we can use the tool `sqlplus` to connect to the Oracle database and interact with it.

