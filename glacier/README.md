# Docs
# [Glacier boto3 initate_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/glacier.html#Glacier.Client.initiate_job)

# [AWS Glacier Workflow](https://www.madboa.com/blog/2016/09/23/glacier-cli-intro/)

# get vault info
```
aws --profile default glacier describe-vault --account-id - --vault-name test_vault
```

## output
```
{
    "VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault",
    "VaultName": "test_vault",
    "CreationDate": "2022-01-18T16:36:49.824Z",
    "NumberOfArchives": 0,
    "SizeInBytes": 0
}
```

# upload an archive
```
aws --profile default glacier upload-archive --account-id - --vault-name test_vault --body shermanteam.tar
```

## output
```
{
    "location": "/164666661898/vaults/test_vault/archives/8Hkbx78mzFwA4rRfGs7Ma7ryeQwGh2yw1TZhqUAWH2qtXlczj5a8WkQEh6o_FUHyESLpbBGrwOMi1ERVw7jnJvCFe5sc8apfOEAYsY2JSzF5nMdHOpZX_qquXx8b-JvVh4nQEJSrew",
    "checksum": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178",
    "archiveId": "8Hkbx78mzFwA4rRfGs7Ma7ryeQwGh2yw1TZhqUAWH2qtXlczj5a8WkQEh6o_FUHyESLpbBGrwOMi1ERVw7jnJvCFe5sc8apfOEAYsY2JSzF5nMdHOpZX_qquXx8b-JvVh4nQEJSrew"
}
```


# list jobs
```
aws --profile default glacier list-jobs --account-id - --vault-name test_vault
```

# inventory retrieval
```
aws --profile default glacier initiate-job \
  --account-id - \
  --vault test_vault \
  --job-parameters '{ "Type": "inventory-retrieval" }'
```

# get job output
```
aws --profile default glacier get-job-output \
  --account-id - \
  --vault-name test_vault \
  --job-id "bbp2xENbArsMfqHHc8prZSKbQRXGlllOkk7NgzM0QpFBD4MKQruocTN5E0xxNcSXOyeOabI2PB0yZyxC6qNPR2qeUIWW" \
  glacier-jobs-out
```