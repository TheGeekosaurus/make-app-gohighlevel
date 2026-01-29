# HighLevel API Notes

Quirks, gotchas, and lessons learned from working with the HighLevel API.

## Authentication

### Token Types
- **Agency Token (Company)**: For agency-level operations
- **Location Token (Sub-Account)**: For most sub-account operations - **this is what most endpoints need**

### Token Exchange Flow
1. User authorizes via `https://marketplace.leadconnectorhq.com/oauth/chooselocation`
2. Exchange code for token at `/oauth/token` with `user_type: "Location"`
3. Tokens expire in **24 hours** (`expires_in: 86399`)
4. Refresh using same endpoint with `grant_type: "refresh_token"`

### Critical: Refresh Token Response
The refresh response includes `locationId`, `companyId`, `userId` - **you must capture these** or they'll be lost after first refresh, breaking any module that needs `locationId`.

## Headers

**Required on ALL requests:**
```
Authorization: Bearer <TOKEN>
Version: 2021-07-28
```

The `Version` header is mandatory - requests without it will fail.

## Media Upload

**Endpoint**: `POST /medias/upload-file`

**Token Type**: Sub-Account (Location) only

**Quirks**:
- `locationId` goes in query string, not body
- Max file size: 25MB (500MB for video)
- Must upload media before referencing in social posts
- Returns `fileId` and `url` on success

## Social Planner

**Endpoint**: `POST /social-media-posting/post`

**Token Type**: Sub-Account (Location) only

**Required Scopes**:
- `socialplanner/post.write`
- `socialplanner/account.readonly` (to list connected accounts)

**Notes**:
- Need to get account IDs first via list accounts endpoint
- Media must be uploaded first, then reference by URL or ID

## Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| 401 "Invalid JWT" | Token expired, refresh failed | Check refresh flow captures all fields |
| 401 Unauthorized | Wrong token type (Agency vs Location) | Use Location token for sub-account endpoints |
| 403 Forbidden | Missing scope | Add required scope and re-authorize |
| 400 Bad Request | Missing required field | Check API docs for required params |

## Useful Endpoints

| Purpose | Endpoint | Method |
|---------|----------|--------|
| Get location info | `/locations/{locationId}` | GET |
| List media files | `/medias/files` | GET |
| Upload media | `/medias/upload-file` | POST |
| List social accounts | `/social-media-posting/{locationId}/accounts` | GET |
| Create post | `/social-media-posting/post` | POST |

## Resources

- [API Reference](https://highlevel.stoplight.io/docs/integrations)
- [OAuth Documentation](https://highlevel.stoplight.io/docs/integrations/a04191c0fabf9-oauth-2-0)
- [Private Integrations Guide](https://help.gohighlevel.com/support/solutions/articles/155000001069-private-integrations)
