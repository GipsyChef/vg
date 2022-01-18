# URL
# https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/glacier.html#Glacier.Client.initiate_job

# get vault info
aws --profile default glacier describe-vault --account-id 164666661898 --vault-name test_vault
{
    "VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault",
    "VaultName": "test_vault",
    "CreationDate": "2022-01-18T16:36:49.824Z",
    "NumberOfArchives": 0,
    "SizeInBytes": 0
}

# upload an archive
aws --profile default glacier upload-archive --account-id 164666661898 --vault-name test_vault --body shermanteam.tar

## output
{
    "location": "/164666661898/vaults/test_vault/archives/8Hkbx78mzFwA4rRfGs7Ma7ryeQwGh2yw1TZhqUAWH2qtXlczj5a8WkQEh6o_FUHyESLpbBGrwOMi1ERVw7jnJvCFe5sc8apfOEAYsY2JSzF5nMdHOpZX_qquXx8b-JvVh4nQEJSrew",
    "checksum": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178",
    "archiveId": "8Hkbx78mzFwA4rRfGs7Ma7ryeQwGh2yw1TZhqUAWH2qtXlczj5a8WkQEh6o_FUHyESLpbBGrwOMi1ERVw7jnJvCFe5sc8apfOEAYsY2JSzF5nMdHOpZX_qquXx8b-JvVh4nQEJSrew"
}



# list jobs
aws --profile default glacier list-jobs --account-id 164666661898 --vault-name test_vault