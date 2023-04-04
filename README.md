# Virtual-Private-Network-for-Workspace

This Java application develops a port forwarding service client. The software accepts command-line options that indicate the host and port to forward to on the destination host. The startForwardClient() method is invoked by the main function once it has read the command-line arguments and completed the following actions:

initiates a handshake with a forward server by calling the method doHandshake().
Following the handshake, the function opens a socket on a port that was chosen at random and waits for a client connection.
The function accepts the connection from the client when it connects to the socket and launches a new thread to transfer data from the client to the destination host.
Program execution lasts until it is stopped.
The following actions are carried out by the doHandshake() function:
connects with the forward server indicated by the command-line options.
sends a client greeting message to the server that includes the client certificate.
the server sends a server hello message with the server certificate.
confirms that a reliable certificate authority signed the server certificate.
connects to the target host and port supplied in the command-line arguments by sending a request to the server.
receives an IV and a session key in a session message from the server.
utilizes the client's private key to decrypt the session key and IV.
creates a static Handshake class that contains the session key and IV.
Throughout operation, the application records a number of messages and failures.
