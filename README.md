<div align="center">
  <img src="https://s.fotora.ru/74039df584dda10d.png" alt="ADVandal Logo" width="200"/>
  
  [![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&duration=2500&pause=800&color=F8A307&center=true&vCenter=true&width=700&lines=%E2%9A%A1+ADVandal;Intercepting+ad+requests...;Monkey-patching+window.b...;Killing+trackers+at+root...;Saving+your+traffic!)](https://git.io/typing-svg)
  
  [![GitHub license](https://img.shields.io/github/license/XPay-Company/ADVandal?style=for-the-badge&color=orange)](https://github.com/XPay-Company/ADVandal/blob/main/LICENSE)
  [![uBlock Origin](https://img.shields.io/badge/uBlock%20Origin-Compatible-purple?style=for-the-badge&logo=ublockorigin)](https://github.com/gorhill/uBlock)
  [![Stage](https://img.shields.io/badge/Stage-Active%20Development-blue?style=for-the-badge)](https://github.com/XPay-Company/ADVandal)
  [![GitHub stars](https://img.shields.io/github/stars/XPay-Company/ADVandal?style=for-the-badge&color=yellow)](https://github.com/XPay-Company/ADVandal/stargazers)
  [![GitHub forks](https://img.shields.io/github/forks/XPay-Company/ADVandal?style=for-the-badge)](https://github.com/XPay-Company/ADVandal/network)
</div>

---

## ⚡ WE DON'T HIDE ADS. WE VANDALIZE THEIR SOURCE CODE.

> **The ad industry evolved. Your blocker should have evolved faster.**

While the world was sleeping, ad networks learned to dance around EasyList. They randomize URLs, obfuscate scripts, and bury trackers under layers of legitimate code. Generic filters are playing whack-a-mole with a hydra. You're still being tracked. Your bandwidth is still being stolen. The ads are just... invisible.

**This ends now.**

ADVandal is not a filter list. It's a surgical instrument for [uBlock Origin](https://github.com/gorhill/uBlock). It injects itself at the kernel of the page load, impersonates native ad objects, and performs a targeted lobotomy on their initialization logic. We don't block the network request—we make sure the request is never born.

---

## 📖 Table of Contents

- [The Manifesto](#-the-manifesto)
- [How It Works (Under the Hood)](#-how-it-works-under-the-hood)
- [Visual Proof](#-visual-proof)
- [Installation](#-installation)
- [Debugging & Live Logs](#-debugging--live-logs)
- [Support & Contribution](#-support--contribution)
- [License](#-license)

---

## 🧠 How It Works (Under the Hood)

Forget CSS rules. We operate at the JavaScript runtime level. ADVandal uses **pre-emptive monkey-patching** at `document-start`—before any other byte of JavaScript is executed.

We have reverse-engineered the obfuscated call stacks of major ad networks (with a current focus on Yandex's ad bundle). ADVandal creates a shadow clone of the ad controller object and replaces every critical method with a void.

```mermaid
graph TD
    A[Page Load] -->|document-start| B(ADVandal Injects)
    B --> C{Interceptor Active}
    C -->|Neutralize| D[b.load → noopFunc]
    C -->|Neutralize| E[b._formatImage → noopFunc]
    C -->|Neutralize| F[b._sendComplete → noopFunc]
    C -->|Monitor| G[new Image()]
    D --> H[Network request TERMINATED]
    E --> H
    F --> I[Telemetry call TERMINATED]
    G --> J[Pixel tracker DESTROYED]
    H --> K((Zero Bytes Sent))
    I --> K
    J --> K
```

The ad logic runs—and hits a concrete wall. The function calls fire, but they execute absolutely nothing. No errors. No DOM breakage. Just perfect, silent annihilation.

---

## 🎯 Target Acquisition

Based on deep analysis of obfuscated ad delivery chains across the English and Russian-speaking web.

| Method / Object | Status | Without ADVandal | Interception Priority |
| :--- | :---: | :--- | :---: |
| `window.b.load` | 🛑 Blocked | Fires the primary banner request | `CRITICAL` |
| `window.b._formatImage` | 🛑 Blocked | Renders the ad container to DOM | `HIGH` |
| `window.b._sendComplete` | 🛑 Blocked | Sends viewability telemetry to the mothership | `HIGH` |
| `new Image()` constructor | 🛡️ Sabotaged | Creates off-DOM tracking pixels to bypass observers | `HIGH` |

> **Why intercept `new Image()`?** Ad networks have evolved to spawn pixel trackers in pure JavaScript memory, completely detached from the DOM tree. Standard DOM observers are blind to this. ADVandal hooks directly into the constructor to choke these ghost pixels at the source.

---

## 🎬 Visual Proof

*Placeholder for video/GIF.*
*Watch the live uBlock Origin logger as ADVandal systematically dismantles ad initialization in real-time.*

<!-- Replace with your actual video/GIF link -->
<!-- [![ADVandal Live Action](https://img.youtube.com/vi/YOUR_VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=YOUR_VIDEO_ID) -->

---

## 📦 Installation

You're a vandal now. Here's how to arm yourself.

### 1. Unlock Advanced User Mode
uBlock Origin's full power is hidden behind a single checkbox.
1.  Open the uBlock Origin Dashboard → **Settings** tab.
2.  Scroll down. Check **“I am an advanced user”**.
3.  Click the gear icon ⚙️ that appears. This is your new playground.

### 2. Load the Vandal Payload
Set the scriptlet resource location.
1.  In the advanced settings, find `userResourcesLocation`.
2.  Change its value to our raw scriptlet URL:
    ```text
    https://raw.githubusercontent.com/XPay-Company/ADVandal/main/src/advandal.js
    ```
3.  Click **“Apply changes”** at the top.

### 3. Deploy the Filter
Link the scriptlet to your target domains.
1.  Go to the **“My filters”** tab.
2.  Add this surgical strike for every domain you want to purify:
    ```adbanner
    yandex.ru##+js(advandal)
    ```
3.  Click **“Apply changes”**.

✅ **Operation complete.** Ad scripts are now neutralized at startup.

---

## 🔍 Debugging & Live Logs

Want to watch the carnage? Open the Browser Console (`F12` → **Console**). ADVandal reports every kill.

```text
[ADVandal] Spy successfully infiltrated the site’s collective!
[ADVandal] Intercepted call to b._formatImage()
[ADVandal] Network request b.load() successfully sent down a dead end.
```

To verify the interceptor is alive and holding, run this snippet in the console:

```javascript
// Should return an empty function body, not native code.
console.log(window.b.load.toString());
```

---

## 🤝 Join the Vandal Crew

This is a war of attrition. Ad networks will adapt. We adapt faster. If you spot a new ad surviving the purge, or a new obfuscation technique:

1.  **Open an Issue** – Describe the ad behavior and the site.
2.  **Attach Intel** – Screenshots of the **Initiator / Call Stack** from DevTools (Network tab) are gold.
3.  **Share Samples** – Any snippet of the obfuscated ad code helps us forge a new counter.

We will update the interception signatures with extreme prejudice.

---

## ⚖️ License

Distributed under the MIT License. See `LICENSE` for more information.
This is a tool of digital self-defense. Use it with the confidence of a righteous vandal.
```
