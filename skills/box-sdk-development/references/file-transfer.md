# File transfer decisions

## Original file outside Box

Use the SDK's file-content download operation to stream original bytes. Stream to disk or the destination service; avoid loading a large file into memory. Check whether the SDK handles the Box download redirect, stream errors, timeouts, and cleanup. Do not create previews or iterate representation pages to obtain the source file.

For multiple complete files, check the ZIP download API and the installed SDK's support. It can reduce per-file API requests when an archive is acceptable. The current API caps an archive at the account's upload limit or 10,000 files, whichever comes first, and recommends staying at or below 25 GB for reliable transfers. Verify these limits, URL lifetime, status and missing-item reporting again when implementing. Partition requests by both count and size; for 25,000 files, a single ZIP is not a valid plan. Treat ZIP URLs as secrets. Reconcile the archive's actual contents and name conflicts with the manifest before marking items complete. If files need independent processing, incremental updates, individual retry, or original folder/name mapping that ZIP complicates, choose bounded streaming downloads instead.

If the destination is a user's browser, consider whether a Box-authorized direct download can avoid relaying bytes through the application server. Select it only when the authorization, URL lifetime, and UX fit. If the requested artifact is a preview, use the appropriate preview or representation endpoint instead of original bytes.

## Uploads

Choose direct upload or an SDK-supported chunked upload based on current size limits and restart needs. Stream from the source. For chunked sessions, persist session/part progress where the API permits and verify commit before treating an upload as complete. Handle same-name conflicts and version uploads intentionally; never silently overwrite or create a duplicate.

## Estimate the request budget

Count discovery pages, content requests or ZIP jobs, status polls, retries, metadata lookups, and verification reads. Compare alternatives by request count, bytes moved, peak memory, failure isolation, archive limits, and time to resume. State these assumptions in the final report for large transfers.

Sources: [download file API](https://developer.box.com/reference/get-files-id-content/), [ZIP download API](https://developer.box.com/reference/post-zip-downloads/), [upload guide](https://developer.box.com/guides/uploads/).
