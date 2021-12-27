
# SSH into a VM

```
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
```

# Create Storage Cloud/Bucket

```
gsutil mb gs://$YOUR_BUCKET_NAME-minecraft-backup
```

# Copy file to Storage Bucket

```
gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://[my_bucket]
```

```
gsutil cp gs://[my_bucket]/*.svg .
```

# View Cloud Storage content
```
gsutil ls gs://[YOUR_BUCKET_NAME]
```

# To get the default access list that's been assigned
```
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl.txt
cat acl.txt
```

# To set the access list to private and verify the results
```
gsutil acl set private gs://$BUCKET_NAME_1/setup.html
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl2.txt
cat acl2.txt
```

# To update the access list to make the file publicly readable
```
gsutil acl ch -u AllUsers:R gs://$BUCKET_NAME_1/setup.html
gsutil acl get gs://$BUCKET_NAME_1/setup.html  > acl3.txt
cat acl3.txt
```

# Generate a CSEK key
For the next step, you need an AES-256 base-64 key.
Run the following command to create a key:
```
python3 -c 'import base64; import os; print(base64.encodebytes(os.urandom(32)))'
```

Rewrite the key for file 1 and comment out the old decrypt key
When a file is encrypted, rewriting the file decrypts it using the decryption_key1 that you previously set, and encrypts the file with the new encryption_key.

You are rewriting the key for setup2.html, but not for setup3.html, so that you can see what happens if you don't rotate the keys properly.

```
gsutil rewrite -k gs://$BUCKET_NAME_1/setup2.html
```


Create a JSON lifecycle policy file
To create a file named life.json, run the following command:

```
nano life.json
```

Paste the following value into the life.json file:

```json
{
  "rule":
  [
    {
      "action": {"type": "Delete"},
      "condition": {"age": 31}
    }
  ]
}
```

gsutil lifecycle set life.json gs://$BUCKET_NAME_1

## Task 6: Enable versioning
View the versioning status for the bucket and enable versioning
Run the following command to view the current versioning status for the bucket:

```
gsutil versioning get gs://$BUCKET_NAME_1
```

The Suspended policy means that it is not enabled.

To enable versioning, run the following command:

```
gsutil versioning set on gs://$BUCKET_NAME_1
```

To verify that versioning was enabled, run the following command:

```
gsutil versioning get gs://$BUCKET_NAME_1
```
