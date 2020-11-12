# Failures

## Test 20

The test failed like this:

```
Test 20 failed: /usr/local/bin/cwl-tes --tes https://csc-tesk-noauth.rahtiapp.fi/ --remote-storage-url ftp://vm2722.kaj.pouta.csc.fi/upload --outdir=/tmp/tmpr9oqsel1 --quiet v1.0/dir3.cwl v1.0/dir3-job.yml
Test directory output
Returned non-zero
[job dir3.cwl] job error:
Error validating output record. location 'ftp://vm2722.kaj.pouta.csc.fi/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785/' ends with '/' but is not a Directory
 in {
    "outdir": {
        "location": "ftp://vm2722.kaj.pouta.csc.fi/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785/",
        "basename": "output_28e3a64e-8d71-4fe9-9db1-926b24fe9785",
        "nameroot": "output_28e3a64e-8d71-4fe9-9db1-926b24fe9785",
        "nameext": "",
        "class": "File",
        "checksum": "sha1$e51d4f8637ef41e28440f09608d9a4f3b8400ef0",
        "size": null
    }
}
Final process status is permanentFail
```

but, the folder exists in the FTP:

```
$ lftp ftp://vm2722.kaj.pouta.csc.fi/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785/ -u tesk-1
cd ok, cwd=/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785
lftp tesk-1@vm2722.kaj.pouta.csc.fi:/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785> ls
-rw-r--r--    1 1001     1001           24 Nov 11 12:40 goodbye.txt
-rw-r--r--    1 1001     1001           13 Nov 11 12:40 hello.txt
lftp tesk-1@vm2722.kaj.pouta.csc.fi:/upload/32814bbe-4b76-4788-9faf-c5886fdd6244/output_28e3a64e-8d71-4fe9-9db1-926b24fe9785>
```

## Test 43

The test failed like this:

```
Test 43 failed: /usr/local/bin/cwl-tes --tes https://csc-tesk-noauth.rahtiapp.fi/ --remote-storage-url ftp://vm2722.kaj.pouta.csc.fi/upload --outdir=/tmp/tmp6cn2jwny --quiet v1.0/cat-from-dir.cwl v1.0/cat-from-dir-job.yaml
Pipe to stdin from user provided local File via a Directory literal
Returned non-zero
[job cat-from-dir.cwl] task id: task-0857090a
[job cat-from-dir.cwl] logs: [TaskLog(start_time=datetime.datetime(2020, 11, 12, 7, 25, 49, tzinfo=tzutc()), end_time=datetime.datetime(2020, 11, 12, 7, 26, 55, tzinfo=tzutc()), metadata={'USER_ID': 'anonymousUser'}, logs=None, outputs=None, system_logs=['Cancelling taskmaster: Got status Failed\n'])]
[job cat-from-dir.cwl] job error:
SYSTEM_ERROR
[job cat-from-dir.cwl] job error:
Error collecting output for parameter 'output':
test:1:1: Did not find output file with glob pattern: '['98e94fb63a19e570c7da29724e113d3e4b226afb']'
Final process status is permanentFail

0 tests passed, 1 failures, 0 unsupported features

1 tool tests failed
```

The Pod log is:

```
$ oc logs task-cb4990f2-inputs-filer-82p7p
Traceback (most recent call last):
  File "/usr/bin/filer", line 10, in <module>
  1 # Failures
    sys.exit(main())
  File "/usr/lib/python3.7/site-packages/tesk_core/filer.py", line 556, in main
    if process_file(args.transputtype, afile):
  File "/usr/lib/python3.7/site-packages/tesk_core/filer.py", line 507, in process_file
    trans = newTransput(scheme)
  File "/usr/lib/python3.7/site-packages/tesk_core/filer.py", line 485, in newTransput
    return fileTransputIfEnabled()
  File "/usr/lib/python3.7/site-packages/tesk_core/filer.py", line 479, in fileTransputIfEnabled
    'CONTAINER_BASE_PATH')
tesk_core.exception.FileProtocolDisabled: 'file:' protocol disabled
To enable it, both 'HOST_BASE_PATH' and 'CONTAINER_BASE_PATH' environment variables must be defined.

```

The JSON input is:

```
{
  "outputs": [
    {
      "name": "stdout",
      "url": "ftp://vm2722.kaj.pouta.csc.fi/upload/21a019d2-22b5-46d1-9907-dc59c051fce5/output_ddb886bc-93b4-436f-8007-2c2e81109d43/98e94fb63a19e570c7da29724e113d3e4b226afb",
      "path": "/home/galvaro/common-workflow-language/v1.0/tmpslq7_dmw/98e94fb63a19e570c7da29724e113d3e4b226afb",
      "type": "FILE"
    },
    {
      "name": "workdir",
      "url": "ftp://vm2722.kaj.pouta.csc.fi/upload/21a019d2-22b5-46d1-9907-dc59c051fce5/output_ddb886bc-93b4-436f-8007-2c2e81109d43/",
      "path": "/home/galvaro/common-workflow-language/v1.0/tmpslq7_dmw",
      "type": "DIRECTORY"
    }
  ],
  "inputs": [
    {
      "name": "dir1",
      "description": "cwl_input:dir1",
      "url": "_:5cb672c1-2c9b-4659-a5eb-24289b3a83ad",
      "path": "/tmp/tmpkwk6af3w/stg1a4045b0-0eaa-4c90-b584-2443d8984ee7/cwl",
      "type": "DIRECTORY"
    }
  ],
  "volumes": [],
  "executors": [
    {
      "apiVersion": "batch/v1",
      "kind": "Job",
      "metadata": {
        "annotations": {
          "tes-task-name": "cat-from-dir.cwl"
        },
        "labels": {
          "job-type": "executor",
          "taskmaster-name": "task-cb4990f2",
          "executor-no": "0",
          "creator-user-id": "anonymousUser"
        },
        "name": "task-cb4990f2-ex-00"
      },
      "spec": {
        "template": {
          "metadata": {
            "name": "task-cb4990f2-ex-00"
          },
          "spec": {
            "containers": [
              {
                "command": [
                  "/bin/sh",
                  "-c",
                  "cat < /tmp/tmpkwk6af3w/stg1a4045b0-0eaa-4c90-b584-2443d8984ee7/cwl/hello.txt > /home/galvaro/common-workflow-language/v1.0/tmpslq7_dmw/98e94fb63a19e570c7da29724e113d3e4b226afb"
                ],
                "env": [
                  {
                    "name": "HOME",
                    "value": "/home/galvaro/common-workflow-language/v1.0/tmpslq7_dmw"
                  },
                  {
                    "name": "TMPDIR",
                    "value": "/tmp/tmpvhn86641"
                  }
                ],
                "image": "python:2.7",
                "name": "task-cb4990f2-ex-00",
                "resources": {
                  "requests": {
                    "memory": "0.008G",
                    "cpu": "1"
                  }
                },
                "workingDir": "/home/galvaro/common-workflow-language/v1.0/tmpslq7_dmw"
              }
            ],
            "restartPolicy": "Never"
          }
        }
      }
    }
  ],
  "resources": {
    "disk_gb": 2.1474843604837712
  }
}
```

## Test 44

Same failure as Test 43

## Test 45

Same failure as Test 43

