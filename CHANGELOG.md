# Changelog

Terraform Enterprise release versions have the format:

```
vYYYYMM-N
```

Where:

* `YYYY` and `MM` are the year and month of the release.
* `N` is increased with each release in a given month, starting with `1`

## v201802-3 (Feb 28, 2018)

APPLICATION LEVEL FEATURES:

* Enable the Module Registry for everyone, but disable unsupported VCS providers (GitLab, Bitbucket Cloud).
* Allow site admin membership to be managed through SAML and make the team name configurable.
* The SAML email address setting was removed in favor of always using `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.
* The SAML `AuthnContextClassRef` is hardcoded to `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport` instead of being sent as a blank value.
* Improved SAML error handling and reporting.
* Adds support for configuring the "including submodules on clone" setting when creating a workspace. Previously this had to be configured after creating a workspace, on the settings page.
* Move workspace SSH key setting from integrations page to settings page in UI.
* Add top bar with controls to plan and apply log viewer when in full screen mode.
* Improvements to VCS respository selection UI when creating a workspace.
* Improvements to error handling in the UI when creating a workspace.
* Remove VCS root path setting from workspaces in UI and API.
* API responses with a 422 status have a new format with better error reporting.
* Remove unused attributes from run API responses: `auto-apply`, `error-text`, `metadata`, `terraform-version`, `input-state-version`, `workspace-run-alerts`.
* Remove the `compound-workspace` API endpoints in favor of the `workspace` API endpoints.
* API responses that contain `may-<action>` keys will have those keys moved into a new part of the response: `actions`.
* Organizations now have names in place of usernames. API responses will no longer serialize a `username` key but will instead serialize a `name`.

APPLICATION LEVEL BUG FIXES:

* Allow workspace admins to read and update which SSH key is associated with a workspace. Previously only workspace owners could do this.
* Reject environment variables that contain newlines.
* Fix a bug that caused the VCS repo associated with a workspace displayed incorrectly for new workspaces without runs and workspaces whose current run used to a promoted configuration version.
* Fix a bug that resulted in plan and apply log output to sometimes be truncated in the UI.
* Fix a bug that prevented some environments and workspaces from being deleted.
* A few small fixes to Sentinel error reporting during creation and policy check in the UI.
* A few small fixes to UI when managing SSH keys.
* Missing translation added for configuration versions uploaded via the API.

APPLICATION LEVEL SECURITY FIXES:

* Upgrade ruby-saml gem to 1.7.0 to address CVE-2017-11428.
* Upgrade sinatra gem to 1.4.8 to address CVE-2018-7212.

## v201802-2 (Feb 15, 2018)

APPLICATION LEVEL FEATURES:
  * Ensure the Archivist storage service sets the `x-amz-server-side-encryption` and `x-amz-server-side-encryption-aws-kms-key-id` HTTP headers on all `PutObject` calls to S3, when a KMS key is configured.
  * Add new `no_proxy` variable to support hosts to exclude from being proxied via `proxy_url`, if configured.

APPLICATION LEVEL BUG FIXES:
  * Fix a bug related to audit logging that prevented webhooks from being handled properly.

CUSTOMER NOTES:
  * Added documentation to `aws-standard/README.md` describing character constraints on user-provided values, such as `db_password`.

## v201802-1 (Feb 6, 2018)

APPLICATION LEVEL FEATURES:
  * Adds support to publish modules from Github and Github Enterprise to the Module Registry
  * Adds the ability to search by keyword and filter the workspace list
  * Improves UI response to permissions by hiding/disabling forms/pages based on user’s access level.
  * Adds audit events to the logs to capture the user identity, operation performed and target resource.
  * Configurations can now be promoted across workspace via the API
  * Improves SAML team mapping to use custom names to perform matching

APPLICATION LEVEL BUG FIXES:
  * Fixes the provider and module name parsing when ‘-’ is used in publishing of modules to the module registry.
  * Fixes broken redirects to external site on bad requests.
  * Fixes bug with triggering runs from Bitbucket Server when a module and workspace are both linked to the same source.
  * Fixes rendering issues in IE11
  * Fixes rendering issue with scroll bars on some input fields in Chrome 40.0.2214.115+ on Windows

## v201801-2 (Jan 18, 2018)

APPLICATION LEVEL BUG FIXES:
  * Increase memory allocation for Terraform Module Registry to prevent forced termination when processing large modules.

## v201801-1 (Jan 12, 2018)

APPLICATION LEVEL BUG FIXES:
  * Fix a bug in the Terraform Module Registry where multiple jobs trying to ingress the same version of the same module concurrently errored and would not be retried

MACHINE IMAGE BUG FIXES:
  * Includes OS-level security updates to address Meltdown (see https://usn.ubuntu.com/usn/usn-3522-1/)

## v201712-2 (Dec 18, 2017)

APPLICATION LEVEL BUG FIXES:
  * Clarify the purpose of organization API tokens

MACHINE IMAGE BUG FIXES:
  * Fix postgres compatibility with the private module registry


## v201712-1 (Dec 7, 2017)

APPLICATION LEVEL FEATURES:
  * Includes new Terraform Enterprise interface featuring Workspaces (see https://www.hashicorp.com/blog/hashicorp-terraform-enterprise-beta for details)

APPLICATION LEVEL BUG FIXES:
  * Properly handle repositories with many webhooks
  * Screens with many elements now use pages to display all data

## v201711-1 (Nov 1, 2017)

APPLICATION LEVEL BUG FIXES:
  * The Bitbucket Server integration no longer sends empty JSON payloads with get requests
  * Force Canceled runs will create a run event so that they no longer appear to be planning in the UI

MACHINE IMAGE BUG FIXES:
  * Increase the capacity of the UI to prevent it being unavailable due
    to starvation.

## v201709-3 (Sep 29, 2017)

MACHINE IMAGE BUG FIXES:
  * Properly write the vault root key envvar file.

## v201709-2 (Sep 28, 2017)

MACHINE IMAGE BUG FIXES:
  * cloud.cfg no longer conflicts with the cloud-init package.
  * Restoring from an older, timestamp based backup no longer hangs when
    there are a large number of backups.

## v201709-1 (Sep 13, 2017)

APPLICATION LEVEL FEATURES:
  * Sends a flag to terraform workers on all new runs to enable filesystem 
    preservation between plan/apply.
  * Add support for Terraform 0.10.

APPLICATION LEVEL BUG FIXES:
  * State uploads now validate lineage values.
  * Fixes a potential race during Terraform state creation.
  * Fixes a subtle bug when loading Terraform states which were created prior
    to having the MD5 checksum in a database column.
  * Gradually migrate all Terraform states out of the Postgres DB and 
    into our storage service, Archivist.

MACHINE IMAGE FEATURES:

  * Add ability to prompt for setup data and store it inside Vault rather than
    store it in S3+KMS (activated via new `local_setup` Terraform option).

TERRAFORM CONFIG FEATURES:

  * Add `local_setup` variable to tell TFE to prompt for setup data on first
    boot and store it within Vault rather than rely on S3+KMS for setup data.
  * Make `key_name` variable optional, allowing for deployments without SSH
    access.

## v201708-2 (Aug 16, 2017)

MACHINE IMAGE BUG FIXES:

  * Correct out of memory condition with various internal services that prevent
    proper operation.

## v201708-1 (Aug 8, 2017)

APPLICATION LEVEL BUG FIXES:

  * Fixes a bug where TF slugs would only get encrypted during terraform push.
  * Fixes state parser triggering for states stored in external storage (Archivist).
  * Fixes a bug where encryption contexts could be overwritten.
  * Send commit status updates to the GitHub VCS provider when plan is "running" (cosmetic)

MACHINE IMAGE BUG FIXES:

  * Manage upgrading from v201706-4 and earlier properly.

## v201707-2 (July 26, 2017)

APPLICATION LEVEL BUG FIXES:

  * Send commit status updates to VCS providers while waiting for MFA input

## v201707-1 (July 18, 2017)

APPLICATION LEVEL FEATURES:

  * Add support for releases up to Terraform 0.9.9.

APPLICATION LEVEL BUG FIXES:

  * Displays an error message if the incorrect MFA code is entered to confirm a Run.
  * Address issue with large recipient groups in new admin notification emails.
  * Fixes a 500 error on the Events page for some older accounts.
  * Fix provider names in new environment form.
  * Update wording in the Event Log for version control linking and unlinking.
  * Fetch MFA credential from private registries when enabled.
  * Fix ability to cancel Plans, Applies, and Runs

MACHINE IMAGE FEATURES:

  * Add ability to use local redis.
  * This adds a new dependency on EBS to store the redis data.

TERRAFORM CONFIG FEATURES:

  * Add `local_redis` variable to configure cluster to use redis locally, eliminating
    a dependency on ElasticCache.
  * Add `ebs_size` variable to configure size of EBS volumes to create to store local
    redis data.
  * Add `ebs_redundancy` variable to number of EBS volumes to mirror together for
    redundancy in storing redis data.
  * Add `iam_role` as an output to allow for additional changes to be applied to
    the IAM role used by the cluster instance.


## v201706-4 (June 26, 2017)

APPLICATION LEVEL FEATURES:

  * Add support for releases up to Terraform 0.9.8.

APPLICATION LEVEL BUG FIXES:

  * VCS: Send commit status updates after every `terraform plan` that has a
    commit.
  * Fix admin page that displays Terraform Runs.
  * Remove application identifying HTTP headers.

MACHINE IMAGE BUG FIXES:

  * Fix `rails-console` to be more usable and provide a command prompt.
  * Fix DNS servers exposed to builds to use DNS servers that are configured
    for the instance.
  * Redact sensitive information from error output generated while talking to
    VCS providers.
  * Refresh tokens for Bitbucket and GitLab properly.
  * Update build status on Bitbucket Cloud PRs.

EXTRAS CHANGES:

  * Parametirez s3 endpoint region used for setup of S3 <=> VPC peering.


## v201706-3 (June 7, 2017)

MACHINE IMAGE BUG FIXES:

  * Exclude all cluster local traffic from using the outbound proxy.

## v201706-2 (June 5, 2017)

APPLICATION LEVEL BUG FIXES:

  * Clear all caches on boot to prevent old records from being used.

MACHINE IMAGE FEATURES:

  * Added `clear-cache` to clear all caches used by the cluster.
  * Added `rails-console` to provide swift access to the Ruby on Rails
    console, used for lowlevel application debugging and inspection.

## v201706-1 (June 1, 2017)

APPLICATION LEVEL FEATURES:

  * Improve page load times.
  * Add support for releases up to Terraform 0.9.6.
  * Make Terraform the default page after logging in.

APPLICATION LEVEL BUG FIXES:

  * Bitbucket Cloud stability improvements.
  * GitLab stability improvements.
  * Address a regression for Terraform Runs using Terraform 0.9.x that
    caused Plans run on non-default branches (e.g. from Pull Requests)
    to push state and possibly conflict with default branch Terraform Runs.
  * Ignore any state included by a `terraform push` and always use state
    within Terraform Enterprise.
  * Prevent `terraform init` from accidentially asking for any input.
  * Allow sensitive variable to be updated.
  * Fix "Settings" link in some cases.

MACHINE IMAGE FEATURES:

  * Automatically scale the number of total concurrent builds up based on
    the amount of memory available in the instance.
  * Add ability to configure instance to send all outbound HTTP and HTTPS
    traffic through a user defined proxy server.

TERRAFORM CONFIG FEATURES:

  * Add `proxy_url` variable to configure outbound HTTP/HTTPS proxy.

DOCUMENTATION CHANGES:

  * Remove deprecated references to Consul environments.
  * Include [Encrypted AMI](docs/encrypt-ami.md) for information on using
    encrypted AMIs/EBS.
  * Add [`network-access`](docs/network-access.md) with information about
    the network access required by TFE.
  * Add [`managing-tool-versions`](docs/managing-tool-versions.md) to document
    usage of the `/admin/tools` control panel.

## v201705-2 (May 23, 2017)

APPLICATION LEVEL CHANGES:

  * Prevent sensitive variables from being sent in the clear over the API.
  * Improve setup UI.
  * Add support for releases up to Terraform 0.9.5.
  * Fix bug that prevented packer runs from having their logs automatically
    display.

MACHINE IMAGE CHANGES:

  * Fix archivist being misreported as failing checks.
  * Add ability to add SSL certificates to be trusted into /etc/ssl/certs.
  * Add awscli to build context for use with local\_exec.
  * Infer the s3 endpoint from the region to support different AWS partitions.
  * Add ability to run custom shell code on the first boot.

TERRAFORM CONFIG CHANGES:

  * Add `startup_script` to allow custom shell code to be injected and run on
    first boot.
  * Allow for customer managed security groups.


DOCUMENTATION CHANGES:

  * Include [documentation](docs/support.md) on sending support information via the
    `hashicorp-support` tool.

## v201705-1 (May 12, 2017)

APPLICATION LEVEL CHANGES:

 * Improve UI performance by reducing how often and when pages poll for new
   results.
 * Add support for releases up to Terraform 0.9.4.
 * Add support for releases up to Packer 0.12.3.
 * Fix the From address in email sent by the system.
 * Allow amazon-ebssurrogate builder in Packer.
 * Handle sensitive variables from `packer push`.
 * Improve speed of retrieving and uploading artifacts over HTTP.
 * Added integrations with GitLab and BitBucket Cloud.
 * Removed Consul and Applications functionality.

MACHINE IMAGE CHANGES:

 * Fix an issue preventing the `hashicorp-support` command from successfully
   generating a diagnostic bundle.
 * Fix ability to handle more complex database paswords.
 * More explicit region utilization in S3 access to support S3 in Govcloud.

TERRAFORM CONFIG CHANGES:

 * Make `region` a required input variable to prevent any confusion from the
   default value being set to an unexpected value. Customers who were not
   already setting this can populate it with the former default: `"us-west-2"`
 * Add ability to specify the aws partition to support govcloud.
 * Reorganize supportive modules into a separate `aws-extra` directory
 * Remove a stale output being referenced in `vpc-base`
 * Work around a Terraform bug that prevented the outputs of `vpc-base` from
   being used as inputs for data subnets.
 * Explicitly specify the IAM policy of the KMS key when creating it.
 * Add an Alias to the created KMS key so it is more easily identifiable AWS
   console.
 * Add ability to start the ELB in internal mode.
 * Specify KMS key policy to allow for utilization of the key explicitly by
   the TFE instance role.
 * Add KMS alias for key that is utilized for better inventory tracking.


## v201704-3

APPLICATION LEVEL CHANGES:

(none)

MACHINE IMAGE CHANGES:

* Properly handle database passwords with non-alphanumeric characters
* Remove nginx's `client_max_body_size` limit so users can upload files larger than 1MB

TERRAFORM CONFIG CHANGES:

* Fix var reference issues when specifying `kms_key_id` as an input
* Add explicit IAM policy to KMS key when Terraform manages it
* Add explicit KMS Key Alias for more easily referencing the KMS key in the AWS Web Console

## v201704-2

APPLICATION LEVEL CHANGES:

(none)

MACHINE IMAGE CHANGES:

* Add `hashicorp-support` script to create an encrypted bundle of diagnostic information for passing to HashiCorp Support
* Switch main SSH username to `tfe-admin` from default `ubuntu`
* Allow AMI to be used in downstream Packer builds without triggering bootstrap behavior

TERRAFORM CONFIG CHANGES:

* Allow `kms_key_id` to be optionally specified as input
* Remove unused `az` variable

## v201704-1

APPLICATION LEVEL CHANGES:

(none)

MACHINE IMAGE CHANGES:

* Disable Consul remote exec
* Install git inside build worker Docker container to facilitate terraform module fetching
* Don't redirect traffic incoming from local build workers

TERRAFORM CONFIG CHANGES:

* Prevent extraneous diffs after RDS database creation

## v201703-2

APPLICATION LEVEL CHANGES:

(none)

MACHINE IMAGE CHANGES:

* Prevent race condition by waiting until Consul is running before continuing boot 
* Ensure that Vault is unsealed when instance reboots

## v201703-1

* Initial release!
