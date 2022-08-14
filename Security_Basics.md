# Security

1. Symmetric Encryption : Same key is used for Encryption and Decryption
2. Asymmetric Encryption: Different Keys (Private and Public)
3. SSH is done using Asymmetric Encryption
4. To generate key pair `ssh-keygen` which generates two files id_rsa and id_rsa.pub[public key]
5. Certificates
  - Public : .crt or .pem
  - Private : .key or -key.pem
6. To generate key pair at specific location `ssh-keygen -t rsa`
7. To setup the password less access to the server.
  - Generate key pair from jumphost
  - Copy the public key generated to the Server's `/.ssh/authorised_keys` file
  - OR use this `ssh-copy-id -i ~/.ssh/mykey.pub thor@app01` thor is user of app01 server

8. To create Certificate Signing Request (CSR)
  - If we can to create CSR with app01.csr name
  - Go to  `/etc/httpd/csr` and run openssl command
  - `sudo openssl req -new -newkey rsa:2048 -nodes -keyout app01.key -out app01.csr`
  - Fill out the necessary values which will generate CSR and private key pair
  - To decrypt CSR : `openssl req -noout -text -in app01.csr`

9. To generate the self-signed Certificate
  - Self-signed Certificates are used when there are no CAs available and we need to use it internally
  - Go to `/etc/httpd/certs`
  - Create Certificate : `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout app01.key -out app01.crt`
  - To read the Certificate : `openssl x509 -noout -in app01.crt  -text`
