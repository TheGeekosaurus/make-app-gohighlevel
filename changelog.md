# Changelog

All notable changes to this app will be documented in this file.

## [1.0.1] - 2026-01-28

### Fixed
- OAuth refresh flow now properly captures `locationId`, `companyId`, `userId` on token refresh
- Added explicit refresh condition `{{connection.expires < now}}`
- Resolved "Invalid JWT" 401 errors on scheduled automation runs

## [1.0.0] - 2026-01-27

### Added
- Initial release
- OAuth 2.0 authentication with Location-level tokens
- Upload Media module - upload files to HighLevel media storage
- Schedule Social Post module - create and schedule social media posts
- Required scopes: `medias.write`, `socialplanner/post.write`, etc.
