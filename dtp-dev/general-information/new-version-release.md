# New version release

There are changes coming within new versions, the following statement will define `breaking changes` and `non-breaking changes` in our future release. The version that has breaking changes will be informed as early as possible and come with detailed integration documents. The deprecated features or fields will be marked as `deprecated` first, an alternative will be introduced and the `deprecated` remove after a grace period (6-9 months).

## Breaking changes

* Removing and/or renaming fields.
* Removing or changing the endpoint (paths).
* Changing types or basic structure of fields (like from a string to an array or an object).
* Fixing HTTP verbs, poor response status code or fixing inconsistencies in the current API designs.

## Non-breaking changes

* Adding new fields (new data or renaming while maintaining the old fields) or introduce new fields in nested structure.
* Adding new APIs or functionalities.
