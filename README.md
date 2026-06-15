<div align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&duration=2500&pause=800&color=F8A307&center=true&vCenter=true&width=700&lines=%E2%9A%A1+ADVandal;Intercepting+ad+requests...;Monkey-patching+window.b...;Killing+trackers+at+root...;Saving+your+traffic!" alt="Typing SVG" />
  
  [![GitHub license](https://img.shields.io/github/license/XPay-Company/ADVandal?style=for-the-badge&color=orange)](https://github.com/XPay-Company/ADVandal/blob/main/LICENSE)
  [![uBlock Origin](https://img.shields.io/badge/uBlock%20Origin-Compatible-purple?style=for-the-badge&logo=ublockorigin)](https://github.com/gorhill/uBlock)
  [![Stage](https://img.shields.io/badge/Stage-Development-blue?style=for-the-badge)](https://github.com/XPay-Company/ADVandal)
  [![GitHub stars](https://img.shields.io/github/stars/XPay-Company/ADVandal?style=for-the-badge&color=yellow)](https://github.com/XPay-Company/ADVandal/stargazers)
  [![GitHub forks](https://img.shields.io/github/forks/XPay-Company/ADVandal?style=for-the-badge)](https://github.com/XPay-Company/ADVandal/network)
</div>

---

## 🧨 Advanced Interception & Annihilation of Yandex Ad Scripts at Initialization

> Cosmetic filters hide ads *after* they load.  
> **ADVandal kills them before a single byte leaves your device.**

ADVandal is a high‑performance [uBlock Origin](https://github.com/gorhill/uBlock) scriptlet that injects itself at `document-start`, impersonates native Yandex objects, and disarms ad‑loading call chains *before* the browser ever sends a network request. No banners. No tracking. No wasted bandwidth.

---

## 📖 Table of Contents

- [How It Works](#-how-it-works-under-the-hood)
- [Targets & Intercepted Methods](#-targets--intercepted-methods)
- [Why ADVandal?](#-why-advandal)
- [Installation](#-installation)
- [Debugging & Live Logs](#-debugging--live-logs)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🧠 How It Works (Under the Hood)

ADVandal leverages **monkey‑patching** – dynamically replacing functions at runtime – combined with deep knowledge of Yandex’s obfuscated ad pipeline.

```mermaid
graph TD
    A[Page Load] -->|document-start| B(ADVandal Injects)
    B --> C{Takeover window.b}
    C -->|Neutralize| D[b.load → noopFunc]
    C -->|Neutralize| E[b._formatImage → noopFunc]
    C -->|Neutralize| F[b._sendComplete → noopFunc]
    D --> G[Network request BLOCKED]
    E --> G
    F --> H[Telemetry call BLOCKED]
    G --> I((Ad never rendered))
    H --> I

The script runs before any other JavaScript on the page. It creates a “shadow” copy of the ad object, patches every critical method with an empty function, and monitors the creation of new Image() instances to prevent sneaky pixel trackers. The result: Yandex’s ad logic executes but does absolutely nothing.

🎯 Targets & Intercepted Methods
Based on reverse‑engineered call stacks of the obfuscated Yandex ad bundle.

Method / Object	Status	What It Does Without ADVandal	Interception Priority
window.b.load	🛑 Blocked	Triggers the main banner request	Critical
window.b._formatImage	🛑 Blocked	Assembles the ad container (rendering)	High
window.b._sendComplete	🛑 Blocked	Sends telemetry & view logs back to server	Medium
new Image()	🛡️ Monitored	Creates off‑DOM tracking pixels	High
Why new Image()? Yandex sometimes creates invisible Image elements in memory to bypass DOM observers. ADVandal silently intercepts the constructor and neutralizes requests made without a real <img> tag.

💡 Why ADVandal?
Approach	Advantage	Limitation
Cosmetic filters (e.g. ##.ad)	Simple, safe	Ads still load, consume bandwidth, track
Network filters (e.g. ||ad.js)	Blocks requests entirely	Easily circumvented by renaming scripts
ADVandal (scriptlet)	Pre‑emptive, works on obfuscated code	Requires deep analysis of ad internals
✅ Completely stops ad requests before they reach the network
✅ Zero overhead after injection – just empty function calls
✅ Eliminates telemetry and viewability logs
✅ Saves traffic on metered connections

📦 Installation
1. Enable Advanced User Mode in uBlock Origin
Open the uBlock Origin Dashboard → Settings tab.

Check “I am an advanced user” at the bottom.

Click the gear icon ⚙️ that appears next to it.

2. Set userResourcesLocation
In the advanced settings, locate userResourcesLocation.

Replace its value with the raw URL of the scriptlet:

text
https://raw.githubusercontent.com/XPay-Company/ADVandal/main/src/advandal.js
Click “Apply changes” at the top of the page.

3. Add the Scriptlet Filter
Go to the “My filters” tab.

Add the following line for every Yandex domain you want to protect:

text
yandex.ru##+js(advandal)
Click “Apply changes”.

✅ Done. All Yandex ad scripts are now neutralized at startup.

🔍 Debugging & Live Logs
To watch the “mole” in action, open the Browser Console (F12 → Console).
On successful interception you’ll see:

text
[ADVandal] Spy successfully infiltrated the site’s collective!
[ADVandal] Intercepted call to b._formatImage()
[ADVandal] Network request b.load() successfully sent down a dead end.
You can also run this snippet to check if the interceptor is alive:

js
console.log( window.b.load.toString() )  // should show "function(){}" (empty)
🤝 Contributing
The project is in active development. If you discover a new call stack or notice that Yandex has changed their obfuscation:

Open an Issue.

Attach a screenshot of the Initiator / Call Stack from DevTools (Network tab).

Describe the ad behavior you still see.

We’ll update the interception signatures promptly.

⚖️ License
This project is distributed under the MIT License.
The code is provided “as is” without warranty of any kind.

<div align="center"> <sub>Built with ❤️ by the community. Star the repo if it saves you from Yandex ads!</sub> </div> ```
