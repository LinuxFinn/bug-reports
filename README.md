# Software QA testing Bug reports

## Bug Reports

### [BNET-001] Account creation fails at the captcha.
*   **Target Application:** Blizzard Battle.net Registration Portal
*   **Severity:** High (Blocks user onboarding)
*   **Testing Type:** Authentication Testing

#### Environment
*   **OS:** CachyOS (Desktop)
*   **Browser:** Google Chrome Version 148.0.7778.167 (Official Build) (64-bit)

*   #### Problem Description
The registration form rejects email addresses from known email-forwarding/burner domains (e.g., `anonaddy.com`). Instead of notifying the user that the email domain is banned, the system falsely triggers a Captcha validation failure. This results in the user being stuck in account creation without knowing what to do.

#### Steps to Reproduce
1. Navigate to the Blizzard Battle.net account registration page.
2. Enter the deatils requested by the account portal.
3. Input an active email using a burner domain (e.g., `exampleuser@[addyusername].anonaddy.com`).
4. Successfully complete the required Captcha puzzle challenge.
5. Click the final registration submission button.

#### Expected Result
The system should either accept the email registration or display an accurate error message (e.g., *"This email provider is not supported. Please use a standard email address."*).

### Actual Result
The page refreshes the Captcha widget and displays a misleading error: 
> *Invalid Response"*
