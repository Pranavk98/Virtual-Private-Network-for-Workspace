Hello!

My Name is Pranav Kulkarni i have created project on VPN for Workspace

Software for virtual private networks created with Java and OpenSSL.


Ways to test it: ##

Launch a netcat and make it listen on the destination port that the client has given.

After the handshake is over, connect a netcat to the client's forward port that it provides in the program's logs.

The two netcats should then be able to communicate via text.

Examine the ForwardThread file to see if the encryption is functioning, then uncomment the corresponding bit of code.

Certifications ##

The server and client certificates must have been signed by the same CA for this software to work.

Create the private key for the CA certificate.

OpenSSL Requirement: "openssl req -new -x509 -newkey rsa:2048 -keyout CAprivatekey.pem -out CA.pem"

Create the CSR for the server/client:

OpenSSL Requirement: "openssl req -out client.csr -new -newkey rsa:2048 -keyout clientprivatekey.pem"

With the CA, sign the CSR:

: "openssl x509 -req -in client.csr -CA" -CAkey CA.pem client.pem privatekey.pem -CAcreateserial

Moreover, we must change a.pem-formatted key into a.der:

"openssl pkcs8 -nocrypt -topk8 -inform PEM -in clientprivatekey.pem -outform DER -out clientprivatekey.der" is the command to use.
How does it operate?

A step-by-step breakdown of how a client and server establish a secure communication connection using this project is provided below.

At the points when each work is finished, the step numbers are mentioned in the code.

ClientThank you
The initial message the client delivers to the server is 1. ClientHello and 2. MessageType
3. Certificate = x509 certificate of the client (as a string)
2. Client certificate is checked by the server
ServerHello 3. (if verified)
Instance 1: MessageType = ServerHello
2. Certificate = x509 certificate of the server
Client checks the server's certificate.
5. The client asks for port forwarding to the destination host and port.
Instance 1: forward
TargetHost is equal to server.kth.se (target server)
TargetPort is 6789. (port of server)

6. If server accepts the destination, a session is established:
1. Produce the session key and IV
2. Use the client's public key to encrypt the session and IV (found in the client's certificate).
3. Establish the socket endpoint and the session communication port.

7. Client session message sent by server

1. MessageType = session 
2. Client's Public Key and AES Key Encrypted (as a string)
3. SessionIV is an encrypted version of AES CTR IV.
4. ServerHost is the host's name that the client should connect to (address of VPN server)
5. ServerPort is the TCP port that the client should use to connect (the VPN server has chosen this port).

8. The handshake is complete and the client has received the session message and information.

9. User and VPN client connect.
1. The VPN client selects the user's internal communication host and port
2. The user establishes a TCP connection using the client's host and port.
3. Data sent by the user is now encrypted and delivered through the VPN client to the VPN server, where it is decrypted and sent to the target.

10. The secure communication channel setup is now finished.