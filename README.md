# Software QA Testing Portfolio

## Bug Reports

### [BNET-001] Account creation fails silently at Captcha checkpoint when using burner emails
*   **Target Application:** Blizzard Battle.net Registration Portal
*   **Severity:** High (Blocks user onboarding)
*   **Testing Type:** Authentication / Boundary Value Testing

#### Environment
*   **OS:** CachyOS (Linux Desktop)
*   **Browser:** Google Chrome Version 148.0.7778.167 (Official Build) (64-bit)

#### Problem Description
The registration form rejects email addresses from known email-forwarding/burner domains (e.g., `anonaddy.com`). Instead of notifying the user that the email domain is banned, the system falsely triggers a Captcha validation failure. This results in the user being stuck in an infinite loop during account creation without clear feedback on how to proceed.

#### Steps to Reproduce
1. Navigate to the Blizzard Battle.net account registration page.
2. Enter the initial details requested by the account portal.
3. Input an active email using a burner domain (e.g., `exampleuser@[addyusername].anonaddy.com`).
4. Successfully complete the required Captcha puzzle challenge.
5. Click the final registration submission button.

#### Expected Result
The system should either accept the email registration or display an accurate validation message (e.g., *"This email provider is not supported. Please use a standard email address."*).

#### Actual Result
The page refreshes the Captcha widget and displays a misleading error: 
> *"Invalid Response"*


### [OSU-001] Beatmap update loop fails to apply new version assets
*   **Target Application:** osu!lazer Client
*   **Severity:** Medium (Prevents online score submission / breaks core gameplay progression)
*   **Testing Type:** Functional / Regression Testing

#### Environment
*   **OS:** CachyOS (Linux Desktop)
*   **Client Version:** osu!lazer Version 2026.439.0

#### Problem Description
The beatmap update mechanism fails to write new map assets to the local database. When a user triggers an update for an outdated beatmap, the client falsely reports a successful download. However, launching the map triggers an in-game warning stating the map is outdated and ineligible for score submission. Returning to the song selection screen resets the map status, forcing the user into an infinite update loop.

#### Steps to Reproduce
1. Launch the osu!lazer client.
2. Navigate to the song selection screen.
3. Identify and select a beatmap that has a pending update notification.
4. Click the **Update** button.
5. Wait for the download indicator to complete and display the "Update Successful" state.
6. Launch/play the newly updated beatmap.
7. Observe the in-game warning message regarding the outdated map status.
8. Exit the gameplay screen back to the song selection menu.

#### Expected Result
The client should successfully overwrite the old beatmap assets with the new file version. Playing the map should not trigger an outdated warning, online scores should submit normally, and the update prompt should disappear from the song selection menu.

#### Actual Result
The client exhibits one of two behaviors depending on asset write success:
1.  **Asset Write Failure (Primary):** The client throws an in-game warning message during gameplay initialization: *"the beatmap does not match the online version. Please update or redownload it. Your score will not be submitted."*
2.  **UI Refresh Failure (Variation):** The assets update successfully, allowing score submission without warnings. However, the song selection menu fails to update its state, leaving the original update prompt visible. 

In both scenarios, returning to the song selection menu traps the user in an infinite update prompt loop.

Upon returning to the song selection menu, the map still displays the original update prompt, trapping the user in the cycle described above.
