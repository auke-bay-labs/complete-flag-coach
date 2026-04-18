# CloudKit Dashboard Setup — One-Time Production Deployment

## Steps

1. Go to https://icloud.developer.apple.com
2. Select container: `iCloud.com.completeflagcoach.app`
3. Switch environment to **Development**
4. Run the app from Xcode on a physical device signed into iCloud
5. Create a team — this auto-creates the `SharedTeam` record type
6. Go back to dashboard → **Schema** → **Record Types** → verify `SharedTeam` exists
7. Go to **Indexes** → add Queryable index on `inviteCode` and `teamID`
8. Click **Deploy Schema to Production**

## Required Record Type: SharedTeam

| Field | Type |
|-------|------|
| teamID | String |
| name | String |
| seasonYear | Int(64) |
| ownerAppleUserID | String |
| inviteCode | String |
| updatedAt | Date/Time |
| playsData | Bytes |
| rosterData | Bytes |

## Required Indexes

| Field | Index Type |
|-------|-----------|
| inviteCode | Queryable |
| teamID | Queryable |
| recordName | Queryable (default) |

## After Deployment
- TestFlight and App Store builds use Production environment
- New record types are NOT auto-created in Production
- Must deploy from Development → Production after any schema changes
