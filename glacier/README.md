# Docs

- [Glacier boto3 initate_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/glacier.html#Glacier.Client.initiate_job)
- [AWS Glacier Workflow](https://www.madboa.com/blog/2016/09/23/glacier-cli-intro/)
- [Initiate Job](https://github.com/awsdocs/amazon-glacier-developer-guide/blob/master/doc_source/api-initiate-job-post.md)
- [Amazon S3 Glacier Re:Freezer](https://github.com/awslabs/amazon-s3-glacier-refreezer)

# required iam policy
```json
{
	"Version": "2012-10-17",
	"Statement": [{
		"Sid": "Stmt1642624281057",
		"Action": [
			"glacier:InitiateJob",
			"glacier:DescribeJob",
			"glacier:ListJobs",
			"glacier:GetJobOutput"
		],
		"Effect": "Allow",
		"Resource": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault"
	}]
}
```

# get vault info
```
aws --profile default glacier describe-vault --account-id - --vault-name test_vault
```

## output
```json
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
```json
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
## output
```json
{
    "JobList": [
        {
            "JobId": "MgqNB4u-FfSgoEhdASnAR0TEoqcLZsHpGYalCgiGS9zfUEpbrOBNy1a7L0sDT16Wl0z5yWG5Xeq7sSsCS9Hd2A_qWgso",
            "Action": "InventoryRetrieval",
            "VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault",
            "CreationDate": "2022-01-20T12:19:46.156Z",
            "Completed": true,
            "StatusCode": "Succeeded",
            "StatusMessage": "Succeeded",
            "InventorySizeInBytes": 2343,
            "CompletionDate": "2022-01-20T16:08:39.889Z",
            "InventoryRetrievalParameters": {
                "Format": "JSON"
            }
        }
    ]
}
```

# inventory retrieval
```
aws --profile default glacier initiate-job \
  --account-id - \
  --vault test_vault \
  --job-parameters '{ "Type": "inventory-retrieval" }'
```

## get job output
```
aws --profile default glacier get-job-output \
  --account-id - \
  --vault-name test_vault \
  --job-id "MgqNB4u-FfSgoEhdASnAR0TEoqcLZsHpGYalCgiGS9zfUEpbrOBNy1a7L0sDT16Wl0z5yWG5Xeq7sSsCS9Hd2A_qWgso" \
  glacier-jobs-out
```
## content of glacier-jobs-out file

```json
{
	"VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault",
	"InventoryDate": "2022-01-19T08:00:42Z",
	"ArchiveList": [{
		"ArchiveId": "a3b11n9z0mx9XdJQeNQhr7jrFuKrwEaJuqAk3OPpPWq281o0VvSL7kqRqWe9u4fQpYzELukCgyZQztrVy-6nPIFoLf4YFOYQ21o5Y6WygvvtTz7VDj2iT9jL2yuchvkFhiSQqvOM6g",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T21:26:59Z",
		"Size": 8822861,
		"SHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56"
	}, {
		"ArchiveId": "w6n8lyTflyUMA7uH_DW4aerwFVrzDkYLzfwdPLLLWUOjNCweenB2hsfe1reEK3Aa-1d_-TBI4A32NB39hJO427Tm9_OReLXEymNJqiP6__a0SFDP6KFEH_iB6bJJWhp_THr2-vYfOA",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T21:45:21Z",
		"Size": 8822861,
		"SHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56"
	}, {
		"ArchiveId": "xI8mAvGJ09R_Nkr4Yyc19Crn5NCOiZiSmmQL5EBbgTJGkw4UhSj7E5Fo2DVRTuDyyZuXyDkcByBrrQh-qACQ-KiLcs0qlGkV_S-Ayf_ucY2qTatWkivBoqrKEhoT4ZSv_P4NNUZHtQ",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T22:17:07Z",
		"Size": 14049280,
		"SHA256TreeHash": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178"
	}, {
		"ArchiveId": "lnrzzBTYEDwcndR7v3fjyVUGOKycWxQ5R9s2SmcdJBu2PpvoeuGuZ6u8ePnc9hzlXv82DaItxJS7tpFVHHF0UqatMzKEY4dKpEBbeItXi45pwHI-DpPjt1meAieBs4jspf546RLxzg",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T23:03:44Z",
		"Size": 14049280,
		"SHA256TreeHash": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178"
	}, {
		"ArchiveId": "_FZmvDpT5kqRtA_QtuOml8jLl35503iDAnHJXjto2DtmlMok0hnv1mtsFyuW6DYI2GBosPbu1PXJ6pnohT30mnGpadtGOtycXHEUJNpPjieQz9v3tLzpxiG6_IS1j0RIFwV1xvTimg",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T23:05:27Z",
		"Size": 14049280,
		"SHA256TreeHash": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178"
	}, {
		"ArchiveId": "2pildduz_OflcWV8pNgNbyMEXC5OIUCE_uIvmigVSPF-Dl8HskSMsJUZ5WhVxVuCIJKhNYQiLDt1_2IGgs49w0DqbWp7BtVkh8UUNRFVzhOQcXYXEcpVUqmic3drIS49eooGREli1A",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T23:05:35Z",
		"Size": 14049280,
		"SHA256TreeHash": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178"
	}, {
		"ArchiveId": "L3fVSalHzxGFvHt7Z1D-gZ9tqrag-5nBLUg7Wcx3GVfZBKGHpjIlZuXYnl4aPyY4mzFD1AdETCV_9F2hTOFRKPZEhEaCJmDWJiVDHL5-kjFBeYMy_bP6LWb03WSAl9IodMvZXqt2GQ",
		"ArchiveDescription": "",
		"CreationDate": "2022-01-18T23:05:50Z",
		"Size": 14049280,
		"SHA256TreeHash": "4b8d2ebdb8acae14787acc7aa64a61eacd220d354e92229984fe28cbe3840178"
	}]
}
```

# archive retrieval command 
[select_job.json](select_job.json) contains the query and s3 output
[retrieve_job.json](retrieve_job.json) contains the the job parameters for local retrieval
*archive-retrieval* type doesn't allow s3 output location so *select* type must be used!
```
aws --profile default glacier initiate-job \
  --account-id - \
  --vault-name test_vault \
  --job-parameters file://retrieve_job.json
```
## output
```json
{
    "location": "/164666661898/vaults/test_vault/jobs/mTuA9TGGyjy0HdC2DC6zmEfmCURFxewUqy9azAT7FEDYWVueEdqiKmOjwP6Z24VB-KDaT-OjLc2WZSP3n73FOD2K4Tbv",
    "jobId": "mTuA9TGGyjy0HdC2DC6zmEfmCURFxewUqy9azAT7FEDYWVueEdqiKmOjwP6Z24VB-KDaT-OjLc2WZSP3n73FOD2K4Tbv"
}
```
# sns notification object
upon job completion an SNS notification with the following object is issued
```json
{
	"Action": "ArchiveRetrieval",
	"ArchiveId": "a3b11n9z0mx9XdJQeNQhr7jrFuKrwEaJuqAk3OPpPWq281o0VvSL7kqRqWe9u4fQpYzELukCgyZQztrVy-6nPIFoLf4YFOYQ21o5Y6WygvvtTz7VDj2iT9jL2yuchvkFhiSQqvOM6g",
	"ArchiveSHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56",
	"ArchiveSizeInBytes": 8822861,
	"Completed": true,
	"CompletionDate": "2022-01-20T22:47:10.630Z",
	"CreationDate": "2022-01-20T19:03:07.742Z",
	"InventoryRetrievalParameters": null,
	"InventorySizeInBytes": null,
	"JobDescription": "Initial retrieval",
	"JobId": "mTuA9TGGyjy0HdC2DC6zmEfmCURFxewUqy9azAT7FEDYWVueEdqiKmOjwP6Z24VB-KDaT-OjLc2WZSP3n73FOD2K4Tbv",
	"RetrievalByteRange": "0-8822860",
	"SHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56",
	"SNSTopic": "arn:aws:sns:us-east-1:164666661898:NotifyMe",
	"StatusCode": "Succeeded",
	"StatusMessage": "Succeeded",
	"Tier": "Standard",
	"VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault"
}
```

# get the data bits
this is only for *archive-retrieval* job type.
this will save the bits as a local shermanteam.tar.gz file
```
aws --profile default  glacier get-job-output \
  --account-id - \
  --vault-name test_vault \
  --job-id "mTuA9TGGyjy0HdC2DC6zmEfmCURFxewUqy9azAT7FEDYWVueEdqiKmOjwP6Z24VB-KDaT-OjLc2WZSP3n73FOD2K4Tbv" \
  shermanteam.tar.gz
```
## output
```json
{
    "checksum": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56",
    "status": 200,
    "acceptRanges": "bytes",
    "contentType": "application/octet-stream"
}
```

# error handling
job list output. 
SNS notification object looks identical to below.
*StatusMessage* contains the detailed error description and *JobOutputPath* has the exact error in the object

```json
 {
    "JobId": "H8oZXDgYTCc5hbjVZgSkep7vz0gQVb2reN7TkdCVAHwQb9Tiw5ez6WD3ONd9EL-mAWuY-dBmHPZyiOopnG-nXXM11qVJ",
    "JobDescription": "Initial retrieval",
    "Action": "Select",
    "ArchiveId": "a3b11n9z0mx9XdJQeNQhr7jrFuKrwEaJuqAk3OPpPWq281o0VvSL7kqRqWe9u4fQpYzELukCgyZQztrVy-6nPIFoLf4YFOYQ21o5Y6WygvvtTz7VDj2iT9jL2yuchvkFhiSQqvOM6g",
    "VaultARN": "arn:aws:glacier:us-east-1:164666661898:vaults/test_vault",
    "CreationDate": "2022-01-20T23:27:15.156Z",
    "Completed": true,
    "StatusCode": "Failed",
    "StatusMessage": "USER_EXCEPTION : Glacier Select was unable to complete your request. Please refer to the error output folder at output/H8oZXDgYTCc5hbjVZgSkep7vz0gQVb2reN7TkdCVAHwQb9Tiw5ez6WD3ONd9EL-mAWuY-dBmHPZyiOopnG-nXXM11qVJ/errors for details.",
    "ArchiveSizeInBytes": 8822861,
    "SNSTopic": "arn:aws:sns:us-east-1:164666661898:NotifyMe",
    "CompletionDate": "2022-01-21T03:18:19.837Z",
    "SHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56",
    "ArchiveSHA256TreeHash": "2169e53315bf890a602edfe4798d92351c26bf74d08a2d2dfd1acf49f28ece56",
    "RetrievalByteRange": "0-8822860",
    "Tier": "Standard",
    "JobOutputPath": "output/H8oZXDgYTCc5hbjVZgSkep7vz0gQVb2reN7TkdCVAHwQb9Tiw5ez6WD3ONd9EL-mAWuY-dBmHPZyiOopnG-nXXM11qVJ/",
    "OutputLocation": {
        "S3": {
            "BucketName": "tabor-glacier-test"
        }
    }
}
```
An exact error inside the object referenced in *JobOutputPath* might look like this:
```
RecordBuilderException.CSVParsingExceptionContext(line=32, column=0, charIndex=7323)
```