# GoHighLevel Make.com Custom App

A custom Make.com integration for GoHighLevel (HighLevel) CRM.

## Overview

This app provides Make.com modules for interacting with the GoHighLevel API, enabling automation of social media posting, media management, and other HighLevel functions.

## Features

- **OAuth 2.0 Authentication** - Location-level token with automatic refresh
- **Upload Media** - Upload files to HighLevel's media storage
- **Schedule Social Post** - Create and schedule social media posts

## Setup

### 1. Create the App in Make.com

1. Go to Make.com → Apps → Create a new App
2. Set up the Base, Connection, and Modules using the JSON files in this repo

### 2. Configure HighLevel

1. Create a Private Integration or OAuth App in HighLevel Developer settings
2. Note your `client_id` and `client_secret`
3. Configure redirect URI to Make.com's OAuth callback

### 3. Required Scopes

```
contacts.readonly
contacts.write
locations.readonly
socialplanner/account.readonly
socialplanner/post.readonly
socialplanner/post.write
socialplanner/tag.readonly
socialplanner/category.readonly
users.readonly
medias.write
```

## File Structure

```
├── base/                       # Base URL and default headers
├── connection/                 # OAuth configuration
│   ├── oauth.json             # Auth, token, refresh flows
│   ├── common.json            # Client credentials
│   ├── parameters.json        # Connection input fields
│   ├── scopes.json            # Available scopes
│   └── default-scopes.json    # Default selected scopes
├── modules/                    # Individual modules
│   ├── upload-media/
│   ├── schedule-post/
│   └── _template/             # Template for new modules
├── rpcs/                       # Remote Procedure Calls (dropdowns)
├── webhooks/                   # Instant triggers (if needed)
└── docs/                       # Documentation and notes
```

## Adding a New Module

1. Copy `modules/_template/` to `modules/your-module-name/`
2. Edit each JSON file according to the HighLevel API endpoint
3. Copy the JSON content to Make.com's module editor

## API Notes

- **Base URL**: `https://services.leadconnectorhq.com`
- **Version Header**: Required on all requests - `Version: 2021-07-28`
- **Token Type**: Most endpoints require Location-level tokens (Sub-Account)
- **Token Expiry**: Access tokens expire in 24 hours; refresh tokens handle renewal

## Troubleshooting

### 401 "Invalid JWT"
- Token expired and refresh failed
- Check that `locationId`, `companyId`, `userId` are captured in refresh response
- Verify refresh condition: `{{connection.expires < now}}`

### 403 Forbidden
- Missing required scope
- Check endpoint documentation for required scopes

### Location vs Agency Token
- Most Sub-Account endpoints require Location tokens
- Use `user_type: "Location"` in token exchange
- Use `chooselocation` authorize URL

## Resources

- [Make.com Custom Apps Documentation](https://developers.make.com/custom-apps-documentation)
- [HighLevel API Documentation](https://highlevel.stoplight.io/docs/integrations)
- [HighLevel OAuth Guide](https://highlevel.stoplight.io/docs/integrations/a04191c0fabf9-oauth-2-0)

## Changelog

See [docs/changelog.md](docs/changelog.md)
