# Run CWL Conformance Tests

**Topic coordinator:** Michael R. Crusoe

[&#8629; All topics][topics-overview]

Design and implement a set of tests that that run [CWL v1.2][cwl-1.2]
[conformance tests][cwl-conformance-tests] against:

* [`cwl-tes`][cwl-tes] => [TESK][tesk]
* [cwl-WES][cwl-wes] => [`cwl-tes`][cwl-tes] => [TESK][tesk]

> Note that [cwl-WES][cwl-wes-req] uses a [forked version of
> `cwl-tes`][cwl-tes-fork] that does not include recent changes in neither the
> upstream [`cwl-tes`][cwl-tes] repository nor [`cwltool`][cwltool]. Thus,
> implementing the latter tests will need fixes on the [`cwl-tes`
> fork][cwl-tes-fork] (to include changes that have been made to the upstream
> repository in the meantime), as well as in [cwl-WES] (to fix errors that
> arise from running the `cwl-tes` version with the latest changes).
>
> A somewhat related issue in this subproject could be the testing and, if
> required, finalization of `s3` support in [`cwl-tes`][cwl-tes], for which
> a [pull request][pr-s3] already exists in the upstream [`cwl-tes`][cwl-tes]
> repository.

```
git checkout https://github.com/common-workflow-language/common-workflow-language/
cd common-workflow-language
https://github.com/common-workflow-language/common-workflow-language/
./run_test.sh RUNNER=cwl-tes EXTRA="--tes YOUR_TES_ENDPOINT --remote-storage-url YOUR_FTP_OR_S3_URL_HERE" -j8 --junit-xml=${PWD}/tesk_required_cwl_v1_0.xml --tags=required
```

Testing with Python 3.7 and the included ``requirements.txt`` with the "required" set of CWL v1.0 conformance tests against the TESK instance at a CSC hosted TESK instance.
> 46 tests passed, 4 failures, 0 unsupported features

The failing tests are 20, 43, 44, 45

```
# to run only the failing tests
./run_test.sh RUNNER=cwl-tes EXTRA="--tes YOUR_TES_ENDPOINT --remote-storage-url YOUR_FTP_OR_S3_URL_HERE" -j8 --junit-xml=${PWD}/tesk_required_cwl_v1_0.xml --tags=required -n 20,43-45
```


[cwl-1.2]: <https://github.com/common-workflow-language/cwl-v1.2/>
[cwl-conformance-tests]: <https://github.com/common-workflow-language/cwl-v1.2/blob/main/CONFORMANCE_TESTS.md>
[cwl-tes]: <https://github.com/ohsu-comp-bio/cwl-tes>
[cwl-tes-fork]: <https://github.com/uniqueg/cwl-tes>
[cwltool]: <https://github.com/common-workflow-language/cwltool>
[cwl-wes]: <https://github.com/elixir-cloud-aai/cwl-WES>
[cwl-wes-req]: <https://github.com/elixir-cloud-aai/cwl-WES/blob/749b9886e8ae83cafe7189705b8978af6b8709f7/requirements.txt#L6>
[pr-s3]: <https://github.com/ohsu-comp-bio/cwl-tes/pull/38>
[tesk]: <https://github.com/EMBL-EBI-TSI/TESK>
[topics-overview]: ../README.md#topics
