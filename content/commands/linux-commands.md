# 👤 Linux User Management Commands

In Linux, user management commands allow you to create, modify, and manage user accounts on the system. Here are some commonly used commands:

| Command | Description |
|---------|-------------|
| `who` | Show who is currently logged in. |
| `sudo adduser username` | Create a new user account with the specified username. |
| `finger` | Display info about all users currently logged in (usernames, login time, terminal, etc.). |
| `sudo deluser USER GROUPNAME` | Remove the specified user from the specified group. |
| `last` | Show recent login history of users. |
| `finger username` | Provide info about a specific user (real name, terminal, idle/login time, etc.). |
| `sudo userdel -r username` | Delete a user account and remove their home directory and files (`-r` = recursive). |
| `sudo passwd -l username` | Lock a user account by disabling their password (prevents login). |
| `su - username` | Switch to another user account with that user’s environment. |
| `sudo usermod -a -G GROUPNAME USERNAME` | Add an existing user to a group (without removing them from other groups). |

---

💡 Tip: Always use `sudo` if modifying users outside your own account.

## Certificates

Extract public key from certificate

```bash
openssl x509 -pubkey -noout -in cert-file.pem > pubkey.txt
```

Extract keypair from pkcs12 `.pfx` file
```bash
# Extract the key-pair
openssl pkcs12 -in sample.pfx -nocerts -nodes -out sample.key

#Get the Private Key from the key-pair
openssl rsa -in sample.key -out sample_private.key

# Get the Public Key from key pair
openssl rsa -in sample.key -pubout -out sample_public.key

# Need to do some modification to the private key -> to pkcs8 format
openssl pkcs8 -topk8 -inform PEM -in sample_private.key -outform PEM -nocrypt
# Copy the output and save it as sample_private_pkcs8.key

```