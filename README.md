<div align="center">

# 🚀 MyShort Ultra | Professional URL Suite

![Maintained by JuniorSir](https://img.shields.io/badge/Maintained%20by-JuniorSir-8b5cf6?style=for-the-badge&logo=github&logoColor=white)
</a>
  &nbsp;&nbsp;
  <a href="https://github.com/juniorsir/Url_shortner">
    <img src="https://img.shields.io/badge/⭐-STAR_PROJECT-FFD700?style=for-the-badge" />
  </a>

<br />

_A high-performance URL shortening engine featuring a premium Glassmorphism UI and Smart App-Routing technology._

</div>

---

MyShort Ultra is a high-performance URL shortening engine built with **Next.js/Vercel**, featuring a premium **Glassmorphism UI**, **JWT Security**, and a **Smart App-Routing Engine** designed to bypass restricted social media browsers (Instagram, TikTok, Facebook).

<div align="center">
  <img src="https://img.shields.io/badge/LIVE%20EXPERIENCE-AVAILABLE%20NOW-8b5cf6?style=for-the-badge&logo=vercel&logoColor=white" />
  <br />
  <p>✨ <b>Experience the Next-Gen Interface</b> ✨</p>
  <a href="https://myshort.vercel.app">
    <b>Web ui available at:</b><br>
    <kbd>https://myshort.vercel.app</kbd>
  </a>
  <br />
  <br />
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%" />
</div>

## ✨ Core Features
- **💎 Glass Theme:** Advanced backdrop blur transparency with dynamic Aura theme switching.
- **📱 App-Aware Redirects:** Automatically triggers native apps (YouTube, Spotify) on mobile devices.
- **🔒 Dual-Layer Auth:** Secure JWT-based access for Owners (Admins) and Sub-Users (API keys).
- **🛠 Multi-Language API:** Programmatic link creation via Python, Node.js, JS, and cURL.
- **📊 Local Analytics:** Persistent history and click tracking stored in the user's browser.
- **🎨 Dynamic Branding:** Real-time Favicon fetching for link previews.

---


## 🛠 Setup & Environment Variables

Deploy this project on Vercel and add these **Environment Variables**:

| Key | Description | Example Value |
| :--- | :--- | :--- |
| `JWT_SECRET` | Secret key used to sign and verify security tokens. | `a_very_long_random_string` |
| `PUBLIC_FORM_ENABLED` | Allow visitors to create links without a token. | `true` |
| `PUBLIC_CUSTOM_ALIAS_ENABLED` | Allow visitors to set their own slug (e.g. /my-link). | `false` |

---

## 🔑 Permissions & Roles

The system uses JWT payloads to enforce different access levels:

| Feature | Owner (Admin) | Sub-User | Public |
| :--- | :---: | :---: | :---: |
| Create Links via UI/API | ✅ | ✅ | ✅ |
| Custom Aliases | ✅ | ✅ | ❌ |
| **Iframe Mode** | ✅ | ❌ | ❌ |
| Manage Ad Slots | ✅ | ❌ | ❌ |
| Manage User Keys | ✅ | ❌ | ❌ |
| Export/Import DB | ✅ | ❌ | ❌ |

---

## 🛰 API Documentation

### 1. Authenticate (`POST /api/auth`)
Exchange a password for a JWT token (Valid for 2 hours).

**Request Body:**
```json
{ "password": "YOUR_PASSWORD" }
```
**Response**
```json
{ "token": "eyJhbGciOiJIUzI1NiIsIn...", "role": "admin" }
```

## Create Smart Links ( POST /api/shorten)

**Header:** ```json Authorization: Bearer <YOUR_TOKEN> ```

### Request Body:
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| long | string | Yes | The destination URL. |
| alias | string | No | Custom slug (3-20 characters). |
| iframe | boolean| No | If true, uses Iframe mode (Admin only). |

## 💻 Language Examples (One-File Integration)
*These examples show how to Authenticate and Shorten in a single execution.*

---

## 🔑 Requesting API Access

Access to create links via the API is restricted to authorized partners and administrators. If you require a unique **Sub-User Password** and **JWT Token**, please reach out through the following official channels:

<div align="center">

| Channel | Contact Link |
| :--- | :--- |
| **Telegram** | [![Telegram](https://img.shields.io/badge/Direct_Message-Blue?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/Junior_sir) |
| **Email** | [![Email](https://img.shields.io/badge/Send_Inquiry-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:juniorsir.bot@gmail.com) |

<br>
<i><b>Note:</b> Please include your project name and expected monthly click volume in your request.</i>
</div>

---
### 🐍 Python (using requests)
```python
import requests

API_BASE = "https://myshort.vercel.app"
PASSWORD = "get_your_password"

def quick_shorten(url, slug=None):
    # 1. Auth
    auth = requests.post(f"{API_BASE}/api/auth", json={"password": PASSWORD}).json()
    token = auth.get("token")
    
    # 2. Shorten
    headers = {"Authorization": f"Bearer {token}"}
    data = {"long": url, "alias": slug}
    res = requests.post(f"{API_BASE}/api/shorten", json=data, headers=headers).json()
    
    print(f"🚀 Short URL: {res.get('short')}")

quick_shorten("https://github.com/juniorsir", "cool-vid")
```
### Dont forget to install axios

```bash 
npm install axios
```

### 🟢 Node.js (using axios)
```javascript
const axios = require('axios');

// --- Configuration ---
const CONFIG = {
    apiBase: "https://myshort.vercel.app",
    password: "your_password_here"
};

async function createSmartLink(longUrl, customAlias = null) {
    try {
        console.log("🔄 Authenticating...");
        
        // 1. Exchange password for a secure JWT Token
        const authResponse = await axios.post(`${CONFIG.apiBase}/api/auth`, {
            password: CONFIG.password
        });
        
        const token = authResponse.data.token;
        console.log("✅ Token Obtained.");

        // 2. Use the token to shorten the URL
        const shortenResponse = await axios.post(
            `${CONFIG.apiBase}/api/shorten`,
            { 
                long: longUrl, 
                alias: customAlias 
            },
            { 
                headers: { 
                    'Authorization': `Bearer ${token}`,
                    'Content-Type': 'application/json'
                } 
            }
        );

        console.log("🚀 Success! Your link is ready:");
        console.log("🔗 Short URL:", shortenResponse.data.short);
        return shortenResponse.data.short;

    } catch (error) {
        // Handle Errors gracefully
        const message = error.response ? error.response.data.error : error.message;
        console.error("❌ Error:", message);
    }
}

// --- Usage ---
createSmartLink("https://github.com/juniorsir", "my-pro-video");
```
### 🖥️ cURL (Terminal/Bash)
```bash
# Get token first
TOKEN=$(curl -s -X POST https://myshort.vercel.app/api/auth \
     -H "Content-Type: application/json" \
     -d '{"password": "your_password"}' | jq -r '.token')

# Create link
curl -X POST https://myshort.vercel.app/api/shorten \
     -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"long": "https://github.com/juniorsir"}'
```

## ⚠️ Error Reference
**• 401 Unauthorized: Token is expired or password is wrong.
• 403 Forbidden: Sub-user attempted to use an Admin-only feature (like Iframe).
• 409 Conflict: The custom alias is already in use.**

# 🤝 Credits & Support
*MyShort Ultra Suite is developed and maintained by JuniorSir.*

<div align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%" />
  
  <h3>🤝 Let's Connect!</h3>
  <p>If you like my projects, feel free to follow me for the latest updates!</p>
  
  <a href="https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fjuniorsir">
    <img src="https://img.shields.io/badge/FOLLOW-ME%20ON%20GITHUB-8b5cf6?style=for-the-badge&logo=github&logoColor=white" />
  </a>
  <a href="https://github.com/juniorsir/Url_shortner">
    <img src="https://img.shields.io/badge/⭐-STAR_PROJECT-FFD700?style=for-the-badge" />
  </a>

  <br><br>
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png" width="100%" />
</div>

## Made with ❤️ by JuniorSir at © 2025 MyShort Ultra Suite
