# Changelog

## [v0.9.2]
- Support reading tokens from Azure's MSAL token cache, as created by azure-cli >= 2.30

## [v0.9.0]
- Default to `az://` output for Azure blobs, instead of `https://`
- Retry "put block list" requests on InvalidBlockList response, because they can actually be
  valid and Azure is just slow to recognise that
- Add missing sleep during read backoff
- Refactoring, in particular, some internal dataclasses are now frozen

## [v0.8.2]
- Fix scrollback for private command `_dud1`
- Make retry logic more consistent

## [v0.8.1]
- Retry broken reads when listing Azure blobs under load
- Use uvloop on Python 3.9, if we have a new enough version

## [v0.8.0]
- Add an `--exclude` (or `-x`) option to `bbb sync`
- `bbb edit` can create new files
- Increase number of request retry attempts
- Fail storage account key fallback in case of 429s due to Azure's really low rate limits
- Paginate Azure requests against subscriptions, in case you have a lot of subscriptions
- Add a private command to do the equivalent of `du -d 1`, live updates in the terminal
- Add some private code for undeleting Azure blobs
- Allow Azure profile to be missing subscriptions
- Make test locations customisable

## [v0.7.0]
- Added disk caching of auth tokens. This speeds up autocomplete (and other quick commands)
  significantly
- Improved handling of concurrent file deletion; ignore during syncing, raise FileNotFoundErrors
  during partial reads
- Avoid (transient) deletion when overwriting large files on Azure
- Make `bbb rmtree` remove directory marker files as well
- Make `BasePath.parent` more pathlib like in how it treats trailing slashes
- Improved combinators in `boost.py`, in particular, added `eagerise`
- Make reads more resilient to ServerTimeoutErrors
- Document aiohttp's tendency to leak file descriptors
- Catch OSError during bad hostname checks, for better behaviour with too many open files
- Improve TokenManager initialisation

## [v0.6.3]
- Temporarily cache bad hostname checks

## [v0.6.2]
- Fix handling of ClientConnectionError in some cases (e.g. during reads)
- Tweak default connection timeouts

## [v0.6.1]
- `bbb edit` will skip re-uploading if the file hasn't changed
- Fix upload of empty files on Azure

## [v0.6.0]
- Add `bbb edit` command
- Set BBB_DEFAULT_CONCURRENCY env var to provide a default concurrency for CLI usage
- Apply backpressure in BoostExecutor, preventing high memory usage in some situations
- Refactored logic in boost.py, added new combinators
- Warn about unfinished asynchronous Azure copy operations, provide command to cancel
- Error in edge case with listing invalid Azure URLs without a container
- Avoid using uvloop on Python 3.9
- Print failed request bodies in debug mode
- Really fix `--concurrency 1`

## [v0.5.3]
- Improved error message for SAS token permission errors
- Improved error message for incorrect storage account keys
- Use blocking writes to avoid memory issues with extremely large files
- Fix for `--concurrency 1`

## [v0.5.2]
- Retry reads that raise aiohttp.ClientPayloadError

## [v0.5.1]
- Revert use of Azure CLI's cached tokens

## [v0.5.0]

- Support container level auth for Azure
- Improved support for globs in copy and remove operations
- `bbb rm` and `bbb cp` now print files they operate on
- Better error for attempting to remove a blob directory
- Use Azure CLI's cached tokens, if present. This often speeds up tab complete
- Improve error message for when we require an OAuth token on Azure
- Workaround edge case for shared key authorisation on Azure due to aiohttp over-cleverness
- Increase default connection timeouts to better support slow connections
- Set BBB_DEBUG=1 env var to get debug level logging and tracebacks
- Fix for edge case of stat failures while long listing (affects long listing of `/` on macOS)
- Fix for deadlock issue with low concurrency and boostables that spawn boostables
- Improve racy dequeue logic in BoostExecutor
- Fix for more reliably providing boosts to boostables after they declare themselves not ready
- Reduce BoostExecutor memory usage
- More tests, in particular for BoostExecutor
- Globbing works better in the presence of directory file markers
- Improved documentation

## [v0.4.0]
- Added tab completion! See README for details
- Extend preliminary globbing support to containers, buckets
- More consistent output when listing local directories
- `copytree` is more permissive about existing directories for Python 3.8 and up
- Minor improvements to Azure auth

## [v0.3.5]
- Fixed issue with racy dequeues in UnorderedBoostable
- Silence errors caused by removal of session during shutdown
- Reduce chunk size to 16MB

## [v0.3.4]

- Add summary for long listing, use human readable sizes
- Add several aliases for list operations (e.g. `bbb ll`)
- Fixed syncing in the presence of directory marker files
- Only log retrying of requests after a couple attempts
- Release responses before yielding in paginated requests
- Fixed Azure auth edge case with service principals
- Documentation improvements

## [v0.3.3]

- Speed up `bbb sync`

## [v0.3.2]

- Preliminary support for wildcards in `bbb ls` and `bbb rm`
- Fix `bbb ls -l <file>` to show info
- Use a per token expiration within each TokenManager

## [v0.3.1]

- Utilise cached subscriptions; speeds up storage key based Azure auth
- Fixed `isdir` for an Azure edge case
- Lower default retry limits
- Better error message for non-existent Azure storage account

## [v0.3.0]

- Support specifying Azure paths as `az://storage_account/container/blob`
- Better error when listing a storage account with no containers
- Set BBB_TRACEBACK env variable to get a traceback on error

## [v0.2.2]

- `bbb ls <file>` now works; it no longer complains about the file not being a directory
- More helpful error message for token refresh failures

## [v0.2.1]

- Fixed shared key request signing
- Adjusted dependency specifications

## [v0.2.0]

- Added `bbb sync`
- Fix for listing directory marker files
- Faster pip installs

## [v0.1.4]

- Improved `--help` output for subcommands
- `bbb ls gs://` will now attempt to list all buckets
- Added short aliases for lstree, cptree, rmtree
- Added this changelog!

## [v0.1.3]

For changes before this, see git :-)
