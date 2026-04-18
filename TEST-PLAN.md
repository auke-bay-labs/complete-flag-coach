# Physical Device Test Plan — Team Join Flow

## Prerequisites
- Device A: iPad (head coach) — signed into Apple ID #1
- Device B: iPhone (parent) — signed into Apple ID #2 (different from #1)
- Both on WiFi
- CloudKit schema deployed to Production (see CLOUDKIT-SETUP.md)
- Latest TestFlight build installed on both

## Test 1: Head Coach Creates Team
1. Open app on iPad
2. Sign in with Apple
3. Tap "Create a Team"
4. Enter team name: "Test Eagles"
5. Tap "Create Team"
6. **Verify:** "Publishing team..." spinner appears
7. **Verify:** Code and QR code appear after publish succeeds
8. **Record the 6-character code**
9. Tap "Share with Team" — verify share sheet opens with pre-written message
10. Tap "Copy Code" — verify clipboard has the code
11. Tap "Done" — verify main app loads with Head Coach tabs

## Test 2: Parent Joins via Code (No Apple ID)
1. Open app on iPhone
2. **Verify:** Sign-in screen shows with "Join with Team Code" button
3. Tap "Join with Team Code"
4. Enter the code from Test 1
5. **Verify:** "Looking up team..." spinner appears
6. **Verify:** App transitions to parent view with read-only tabs
7. **Verify:** Playbook tab shows the same plays as the iPad
8. **Verify:** Game Day tab shows the scoreboard (read-only)

## Test 3: Parent Joins via QR Code
1. Delete and reinstall app on iPhone
2. Tap "Join with Team Code"
3. Tap the QR scan button
4. Scan the QR code from the iPad's Team Management screen
5. **Verify:** Code auto-fills
6. Tap "Join as Parent"
7. **Verify:** Joins successfully

## Test 4: Wrong Code
1. Delete and reinstall app on iPhone
2. Tap "Join with Team Code"
3. Enter "XXXXXX"
4. Tap "Join as Parent"
5. **Verify:** "Team not found" error with specific message
6. **Verify:** "Try Again" button works

## Test 5: Live Scoreboard Sync
1. On iPad (head coach): Go to Game Day > Live
2. Tap Start clock
3. On iPhone (parent): Go to Game Day
4. **Verify:** Clock is ticking on both devices in sync
5. On iPad: Tap +6 for a touchdown
6. **Verify:** iPhone score updates
7. On iPad: Use a timeout
8. **Verify:** Clock stops on both devices, timeout dots update

## Test 6: Apple Watch Sync
1. On iPad: Start clock, change score
2. On paired Apple Watch: Open Complete Flag Coach
3. **Verify:** Watch shows current score and ticking clock
4. On Watch: Tap +6
5. **Verify:** iPad score updates

## Test 7: Roster Visibility
1. On iPad: Go to Roster tab, add a player (name, #, position)
2. Wait 30 seconds for CloudKit sync
3. On iPhone (parent): Check if roster is visible
4. **Verify:** Player appears with correct info
5. **Verify:** Parent cannot edit or add players (no + button, no edit button)

## Test 8: Permission Gating
1. On iPhone (parent):
   - **Verify:** No "Custom" tab (can't create plays)
   - **Verify:** No edit/delete in playbook context menus
   - **Verify:** No score buttons on Game Day scoreboard
   - **Verify:** No gear icon on scoreboard
   - **Verify:** No "Roster" tab
   - **Verify:** "Player" tab exists (position picker)

## Test 9: Game Settings
1. On iPad: Tap gear icon on scoreboard
2. Change quarter length to 12 minutes
3. Change to 2 halves
4. Change timeouts to 2
5. Save
6. **Verify:** Clock shows 12:00
7. Reset game
8. **Verify:** 2 timeout dots, 12:00 clock

## Test 10: Offline Resilience
1. Turn on Airplane Mode on iPad
2. Create plays, change scores
3. Turn off Airplane Mode
4. **Verify:** Data syncs after reconnection

## Pass/Fail Criteria
- All 10 tests must pass
- No crashes
- No data loss
- Sync completes within 30 seconds on WiFi
