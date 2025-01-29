1. Try to connect to the remote machine (if first time connecting to that machine, answer to the question, and press enter three times): 
   `$ ssh remote_username@remote_server_ip_address`
2. Get back to your local machine and execute the following command:
   `$ ssh-keygen`
   This will generate a key-pair public key file in “~/.ssh/id_rsa.pub” and a private key file in “~/.ssh/id_rsa”
3. Now copy your key to the remote machine with:
   `$ ssh-copy-id remote_username@remote_server_ip_address`
4. Login to your remote machine to try the ssh keys with:
   `$ ssh remote_username@remote_server_ip_address`

   
