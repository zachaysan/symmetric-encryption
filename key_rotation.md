---
layout: default
---

## Key Rotation

In order to meet PCI compliance requirements encryption keys must be rotated
on a regular basis. For example, annually.

Below are the steps to perform key rotation while the system is running and to
avoid downtime

### 1. Add the new key as secondary key

During a rolling deploy it is possible for servers to encrypt data using a new
key before the other servers have been updated. This would result in cipher
errors should any of the servers try to decrypt the data since they do not have
the key.

To avoid this race-condition add the new key as the second key in the configuration
file. That way it will continue decrypting using the current key, but can also
decrypt with the new key during the rolling deploy.

### 2. Activate the new key

Once the new key has been deployed as a secondary key, the next deploy can move
the new key to the top of the list so that it can also start encrypting with the
new key.
Keep the old key as the second key in the list so that it can continue to
decrypt old data using the old key.

### 3. Re-encrypting existing data

For PCI Compliance it is necessary to re-encrypt old data with the new key and
then to destroy the old key so that it cannot be used again.

[RocketJob](http://rocketjob.io) is an excellent solution for
running large batch jobs that need to process millions of records.

### 4. Re-encrypting Files

Remember to re-encrypt any files that were encrypted with Symmetric Encryption
if they need to be kept after the old encryption key has been destroyed.

### 5. Remove old key from configuration file

Once all data and files have been re-encrypted using the new key, remove the
old key from the configuration file. If you get cipher errors, you can restore
the old key in the configuration file and then re-encrypt that data too.

### 6. Destroying old key

Once sufficient time has passed and you are 100% certain that there is no data
around that is still encrypted with the old key, wipe the key from all the production
servers.

### Next => [PCI Compliance](pci_compliance.html)
