# S25 Migration: Reconnection and Configuration Guide

## 1. Google Voice
- **Problem:** Device link often breaks during migration.
- **Action:** Open Google Voice on S25. Go to **Settings > Devices and Numbers**. Remove the old "S20 FE" and ensure the new "S25" is toggled **ON** for receiving calls.

## 2. WhatsApp
- **Problem:** Chats and account do not transfer automatically for security.
- **Action:** Insert SIM card. Open WhatsApp. Restore from Google Drive backup.

## 3. Garmin Fenix 7X
- **Problem:** Watches must be re-paired via the app.
- **Action:** Remove watch from old phone; set Fenix 7X to **Pair Phone** mode; add via Garmin Connect on S25.

## 4. Duo Mobile MFA
- **Problem:** MFA tokens are device-specific.
- **Action:** Use "Duo Restore" or LBL Identity portal to add S25.

## 5. Google TV (Exposure Management)
- **Problem:** Synced work and personal profiles.
- **Action:** Open Google TV > Profile icon > Manage accounts. Ensure active profile is `mamiller@gmail.com`.

## 6. Digital Wallets
- **Action:** Re-verify cards using CVV and Bank SMS. Move transit passes (Clipper) manually.

## 7. Photo Backup & Bloat Control
- **Dropbox:** Camera uploads DISABLED. Permissions restricted.
- **OneDrive:** Samsung native sync. If redundant with Google/Amazon, consider disabling the OneDrive app to save battery.
- **Lexar SD Card:** Set as default storage for Camera and offline media.

## 8. Passkey Strategy
- **Service:** Dropbox (and others encouraging passkeys).
- **Provider:** Choose a vault (Google, Samsung, or independent like Bitwarden).
- **Advantage:** Eliminates phishable passwords; allows disabling of SMS MFA.

## 9. App Sync & Subscription Hygiene
- **Journey:** Subscription OFF. Login is with **mamillerpa@gmail.com**. Sync set to **WiFi-only** to save data/battery.
- **General Rule:** Set non-essential sync apps to WiFi-only to reduce background bloat.
