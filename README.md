# Voice-Assistant-Chatbot
# 🎙️ Smart Voice Assistant Chatbot - Fixing & Deployment

A web-based voice assistant chatbot integrated with Google Gemini API via PHP backend. This repository documents the server-side bug fixes, cURL SSL transport configuration, and deployment structure.

---

## 🔍 Identified Issues & Root Cause Analysis

When sending a prompt through the interface, the app returned the following server error:
> **"حدث خطأ أثناء الاتصال بالخادم، حاول مجدداً"**

Upon code review, three main issues were identified in `ro.php` and `config.php`:

1. **Incorrect Relative Path (`require` Error):**
   * **Issue:** `ro.php` tried to include `config.php` via `require __DIR__ . '/../config.php';` (assuming a subfolder structure like `/api/chat.php`).
   * **Fix:** Updated the path to `require __DIR__ . '/config.php';` since all files reside in the same root directory.

2. **Unset Gemini API Key:**
   * **Issue:** `config.php` contained an empty constant `define('GEMINI_API_KEY', '');`.
   * **Fix:** Updated `config.php` and added guard conditions in `ro.php` to handle valid API key definitions.

3. **cURL SSL Handshake Failure (`cURL error 60`) on Shared Hosting:**
   * **Issue:** Strict SSL verification (`CURLOPT_SSL_VERIFYPEER => true`) caused network call failures on InfinityFree environment due to missing CA bundles.
   * **Fix:** Disabled SSL peer/host verification inside cURL options (`CURLOPT_SSL_VERIFYPEER => false` & `CURLOPT_SSL_VERIFYHOST => false`).

---

## 📂 Project Structure

```text
├── index.html        # Main user interface (Voice microphone & chat view)
├── style.css         # Styling and dark layout animations
├── app.js            # JavaScript Web Speech API handler and fetch request
├── config.php        # Gemini API Key configuration
├── ro.php            # Fixed PHP backend script communicating with Gemini API
└── .htaccess         # Security configuration rules
🛠️ Summary of Code Changes
⁠config.php⁠
<?php
// config.php - Gemini API Key Configuration
define('GEMINI_API_KEY', 'YOUR_API_KEY_HERE');
?>

ro.php⁠ (Key Changes)
 Changed file inclusion path to ⁠./config.php⁠.
 Enabled SSL bypass options for cURL connection compatibility with InfinityFree servers.

🚀 How to Run / Deploy
1. Upload all files (⁠index.html⁠, ⁠style.css⁠, ⁠app.js⁠, ⁠config.php⁠, ⁠ro.php⁠, ⁠.htaccess⁠) to your server (⁠htdocs⁠ / ⁠public_html⁠).
2. Insert a valid Gemini API Key inside ⁠config.php⁠.
3. Open ⁠index.html⁠ in a Web Speech API supported browser (Google Chrome or Microsoft Edge).



