# Personal proxy-fix releases

This branch is based on upstream v2.7.1 with the HTTP proxy fix from NetApp/terraform-provider-netapp-ontap#751. Releases are published personally by chris-cmsoft; this is not an official NetApp or Laravel distribution.

The fork's default branch is release-proxy. Its only workflow is manually dispatched and every release job requires chris-cmsoft's approval through the release environment. All third-party actions are pinned to commit SHAs. The upstream contribution branch remains separate.

## Publish a version

1. Run the HTTP client regression tests and review the diff against v2.7.1.
2. Create and push a tag such as v2.7.1-proxy.1 on the reviewed release commit.
3. Run "Build signed provider release" from release-proxy, supplying that tag.
4. Approve the release environment job in GitHub Actions after checking the tag and commit.
5. Review the draft release's ZIPs, SHA256SUMS, detached signature, and manifest. Publish the draft only after the workflow succeeds.
6. Confirm Terraform Registry ingested the release and verify installation using chris-cmsoft/netapp-ontap with an exact version constraint.

The workflow uses GPG_PRIVATE_KEY and PASSPHRASE secrets and checks the imported key against B1B18BE58DA3FCD4. It builds linux, darwin, and windows packages for amd64 and arm64. No live ONTAP acceptance tests are run. Existing TestRequest_BuildHTTPReq failures on the upstream base are outside this patch; the workflow runs HTTP client tests and go vet.

Keep the repository available while consuming releases. Once upstream publishes the fix and consumers migrate, disable Actions and remove the signing secrets before considering archival.
