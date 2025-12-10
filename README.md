# 🎵 Music Clips Collection

A **professional, lightweight collection** of short music audio clips (≤ 30 seconds), stored in the `Audios/` folder and optimized for **easy fetching, streaming, and bot/app usage**.

No APIs.  
No filenames required.  
Just fetch **random audio clips** directly from GitHub.

---

## 📁 Repository Overview

Music-Clips-Collection/ └── Audios/ └── (audio files only)

✅ Only audio files  
✅ Best compatibility formats  
✅ Very small file sizes  
✅ Easy to integrate anywhere  

---

## 🔗 Base Concept (Important)

This repository uses the **GitHub Contents API** to:
1. List all files inside `Audios/`
2. Randomly select one file
3. Use its `download_url` to stream or download

**Contents API Endpoint**

https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main

---

## 🟢 Example 1 — JavaScript (Node.js)  
### Fetch a Random Audio URL

```js
async function getRandomAudio() {
  const res = await fetch(
    "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main"
  );

  if (!res.ok) {
    throw new Error("Failed to fetch audio list");
  }

  const files = await res.json();
  const randomFile = files[Math.floor(Math.random() * files.length)];

  return randomFile.download_url;
}

// Preview
getRandomAudio().then(console.log).catch(console.error);

✅ Copy-paste ready
✅ No filenames used
✅ Returns raw audio URL


---

🟢 Example 2 — JavaScript (Browser)

Play a Random Audio Clip

<button onclick="playRandomAudio()">Play Random Audio</button>
<audio id="audioPlayer" controls></audio>

<script>
async function playRandomAudio() {
  const res = await fetch(
    "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main"
  );

  const files = await res.json();
  const randomFile = files[Math.floor(Math.random() * files.length)];

  const player = document.getElementById("audioPlayer");
  player.src = randomFile.download_url;
  player.play();
}
</script>

✅ Perfect for websites
✅ Lightweight streaming
✅ User-friendly preview


---

🟢 Example 3 — JavaScript (Bot / Backend)

Download a Random Audio File

import fs from "fs";
import fetch from "node-fetch";

async function downloadRandomAudio() {
  const list = await fetch(
    "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main"
  );
  const files = await list.json();

  const randomFile = files[Math.floor(Math.random() * files.length)];
  const audio = await fetch(randomFile.download_url);
  const buffer = await audio.arrayBuffer();

  fs.writeFileSync("random-audio", Buffer.from(buffer));
}

downloadRandomAudio();

✅ Useful for bots
✅ Saves local file
✅ Works with any audio format


---

🟢 Example 4 — curl

Download a Random Audio (Terminal / Termux)

curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
| jq -r '.[].download_url' \
| shuf -n 1 \
| xargs -I{} curl -L "{}" -o random-audio

✅ One-command usage
✅ Great for automation
✅ No hardcoded filenames


---

🟢 Example 5 — Print Random Audio URL Only

curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
| jq -r '.[].download_url' | shuf -n 1

✅ Useful for bots & scripts
✅ Returns clean raw URL


---

🟢 Example 6 — Stream in Terminal (mpv)

URL=$(curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
| jq -r '.[].download_url' | shuf -n1)

mpv "$URL"

✅ Instant preview
✅ No download needed


---

💡 Use Cases

Discord / WhatsApp bots

Mobile & web apps

Music previews

Sound testing

Lightweight streaming



---

⚠️ Notes

GitHub API has rate limits (public use is usually enough).

Only audio files should be inside the Audios/ folder.

The system auto-supports new uploads — no code changes needed.



---

👤 Credits

Created & maintained by

𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧


---

⭐ Support & Follow



If this repo helps you, please ⭐ star it!


---

🎧 Simple. Random. and Powerful ;)