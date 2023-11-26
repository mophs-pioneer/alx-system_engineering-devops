Generating an SSH Key Pair
Generating a new SSH public and private key pair on your local computer is the first step towards authenticating with a remote server without a password. Unless there is a good reason not to, you should always authenticate using SSH keys.

A number of cryptographic algorithms can be used to generate SSH keys, including RSA, DSA, and ECDSA. RSA keys are generally preferred and are the default key type.

To generate an RSA key pair on your local computer, type:

ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/demo/.ssh/id_rsa):
This prompt allows you to choose the location to store your RSA private key. Press ENTER to leave this as the default, which will store them in the .ssh hidden directory in your user’s home directory. Leaving the default location selected will allow your SSH client to find the keys automatically.

Enter passphrase (empty for no passphrase):
Enter same passphrase again:
The next prompt allows you to enter a passphrase of an arbitrary length to secure your private key. By default, you will have to enter any passphrase you set here every time you use the private key, as an additional security measure. Feel free to press ENTER to leave this blank if you do not want a passphrase. Keep in mind though that this will allow anyone who gains control of your private key to login to your servers.

If you choose to enter a passphrase, nothing will be displayed as you type. This is a security precaution.

Output
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
8c:e9:7c:fa:bf:c4:e5:9c:c9:b8:60:1f:fe:1c:d3:8a root@here
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|                 |
|       +         |
|      o S   .    |
|     o   . * +   |
|      o + = O .  |
|       + = = +   |
|      ....Eo+    |
+-----------------+
This procedure has generated an RSA SSH key pair, located in the .ssh hidden directory within your user’s home directory. These files are:

~/.ssh/id_rsa: The private key. DO NOT SHARE THIS FILE!
~/.ssh/id_rsa.pub: The associated public key. This can be shared freely without consequence.
Generate an SSH Key Pair with a Larger Number of Bits
SSH keys are 2048 bits by default. This is generally considered to be good enough for security, but you can specify a greater number of bits for a more hardened key.

To do this, include the -b argument with the number of bits you would like. Most servers support keys with a length of at least 4096 bits. Longer keys may not be accepted for DDOS protection purposes:

ssh-keygen -b 4096
If you had previously created a different key, you will be asked if you wish to overwrite your previous key:

Overwrite (y/n)?
If you choose “yes”, your previous key will be overwritten and you will no longer be able to log in to servers using that key. Because of this, be sure to overwrite keys with caution.

Removing or Changing the Passphrase on a Private Key
If you have generated a passphrase for your private key and wish to change or remove it, you can do so easily.

Note: To change or remove the passphrase, you must know the original passphrase. If you have lost the passphrase to the key, there is no recourse and you will have to generate a new key pair.

To change or remove the passphrase, simply type:

ssh-keygen -p
Enter file in which the key is (/root/.ssh/id_rsa):
You can type the location of the key you wish to modify or press ENTER to accept the default value:

Enter old passphrase:
Enter the old passphrase that you wish to change. You will then be prompted for a new passphrase:

Enter new passphrase (empty for no passphrase):
Enter same passphrase again:
Here, enter your new passphrase or press ENTER to remove the passphrase.

Displaying the SSH Key Fingerprint
Each SSH key pair share a single cryptographic “fingerprint” which can be used to uniquely identify the keys. This can be useful in a variety of situations.

To find out the fingerprint of an SSH key, type:

ssh-keygen -l
Enter file in which the key is (/root/.ssh/id_rsa):
You can press ENTER if that is the correct location of the key, else enter the revised location. You will be given a string which contains the bit-length of the key, the fingerprint, and account and host it was created for, and the algorithm used:

Output
4096 8e:c4:82:47:87:c2:26:4b:68:ff:96:1a:39:62:9e:4e  demo@test (RSA)
Copying your Public SSH Key to a Server with SSH-Copy-ID
To copy your public key to a server, allowing you to authenticate without a password, a number of approaches can be taken.

If you currently have password-based SSH access configured to your server, and you have the ssh-copy-id utility installed, this is a simple process. The ssh-copy-id tool is included in many Linux distributions’ OpenSSH packages, so it very likely may be installed by default.

If you have this option, you can easily transfer your public key by typing:

ssh-copy-id username@remote_host
This will prompt you for the user account’s password on the remote system:

The authenticity of host '111.111.11.111 (111.111.11.111)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
demo@111.111.11.111's password:
After typing in the password, the contents of your ~/.ssh/id_rsa.pub key will be appended to the end of the user account’s ~/.ssh/authorized_keys file:

Output
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'demo@111.111.11.111'"
and check to make sure that only the key(s) you wanted were added.
You can now log in to that account without a password:

ssh username@remote_host
Copying your Public SSH Key to a Server Without SSH-Copy-ID
If you do not have the ssh-copy-id utility available, but still have password-based SSH access to the remote server, you can copy the contents of your public key in a different way.

You can output the contents of the key and pipe it into the ssh command. On the remote side, you can ensure that the ~/.ssh directory exists, and then append the piped contents into the ~/.ssh/authorized_keys file:

cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
You will be asked to supply the password for the remote account:

The authenticity of host '111.111.11.111 (111.111.11.111)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
demo@111.111.11.111's password:
After entering the password, your key will be copied, allowing you to log in without a password:

ssh username@remote_IP_host
Copying your Public SSH Key to a Server Manually
If you do not have password-based SSH access available, you will have to add your public key to the remote server manually.

On your local machine, you can find the contents of your public key file by typing:

cat ~/.ssh/id_rsa.pub
Output
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCqql6MzstZYh1TmWWv11q5O3pISj2ZFl9HgH1JLknLLx44+tXfJ7mIrKNxOOwxIxvcBF8PXSYvobFYEZjGIVCEAjrUzLiIxbyCoxVyle7Q+bqgZ8SeeM8wzytsY+dVGcBxF6N4JS+zVk5eMcV385gG3Y6ON3EG112n6d+SMXY0OEBIcO6x+PnUSGHrSgpBgX7Ks1r7xqFa7heJLLt2wWwkARptX7udSq05paBhcpB0pHtA1Rfz3K2B+ZVIpSDfki9UVKzT8JUmwW6NNzSgxUfQHGwnW7kj4jp4AT0VZk3ADw497M2G/12N0PPB5CnhHf7ovgy6nL1ikrygTKRFmNZISvAcywB9GVqNAVE+ZHDSCuURNsAInVzgYo9xgJDW8wUw2o8U77+xiFxgI5QSZX3Iq7YLMgeksaO4rBJEa54k8m5wEiEE1nUhLuJ0X/vh2xPff6SQ1BL/zkOhvJCACK6Vb15mDOeCSq54Cr7kvS46itMosi/uS66+PujOO+xt/2FWYepz6ZlN70bRly57Q06J+ZJoc9FfBCbCyYH7U/ASsmY095ywPsBo1XQ9PqhnN1/YOorJ068foQDNVpm146mUpILVxmq41Cj55YKHEazXGsdBIbXWhcrRf4G2fJLRcGUr9q8/lERo9oxRm5JFX6TCmj6kmiFqv+Ow9gI0x8GvaQ== demo@test
You can copy this value, and manually paste it into the appropriate location on the remote server. You will have to log in to the remote server through other means (like the DigitalOcean web console).

On the remote server, create the ~/.ssh directory if it does not already exist:

mkdir -p ~/.ssh
Afterwards, you can create or append the ~/.ssh/authorized_keys file by typing:

echo public_key_string >> ~/.ssh/authorized_keys
You should now be able to log in to the remote server without a password.
