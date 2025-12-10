# 🎵 Music Clips Collection

A **simple** repo of very short music audio clips (≤ 30s) stored in `Audios/`.  
This README shows **copy-pasteable examples** to fetch a **random audio file** from the `Audios` folder — **no filenames required**.

> Repo: `xhclintohn/Music-Clips-Collection`  
> Folder: `Audios/`  
> Creator / Credits: **𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧**

---

[![Follow Me](https://img.shields.io/github/followers/xhclintohn?label=Follow%20Developer&style=for-the-badge)](https://github.com/xhclintohn)

---

## How it works (short)
1. Use the GitHub Contents API to list files in `Audios/`.  
   `GET https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main`
2. Pick one random entry from the returned JSON array.
3. Use the `download_url` field of that entry to download or stream the audio.

---

## ✅ Notes
- No API key is required for public repos (rate limits apply for unauthenticated use).
- Examples below are intentionally minimal and copy-paste friendly.
- Files in `Audios/` can be any audio format (mp3, ogg, m4a...). The `download_url` will point to the raw file.

---

## 1) Fetch a random audio — **Node.js (recommended)**

```js
// save as fetch-random-audio.js (Node 18+ or with node-fetch polyfill)
import fetch from "node-fetch"; // Node 18+ has fetch builtin; otherwise install node-fetch

async function getRandomAudio() {
  const api = "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main";
  const res = await fetch(api);
  if (!res.ok) throw new Error(`GitHub API error: ${res.status}`);
  const files = await res.json();                 // array of items
  if (!Array.isArray(files) || !files.length) throw new Error("No audio files found.");
  const rand = files[Math.floor(Math.random() * files.length)];
  return rand.download_url;                       // direct raw file URL
}

(async () => {
  try {
    const url = await getRandomAudio();
    console.log(url); // use this URL to stream or download
    // Example: in Node you can download it:
    // const out = await fetch(url); const buffer = await out.arrayBuffer(); require('fs').writeFileSync('random-audio.mp3', Buffer.from(buffer));
  } catch (e) {
    console.error(e);
  }
})();


---

2) Fetch & play random audio — Browser (vanilla JS)

<!-- paste into an HTML file and open in browser -->
<button id="play">Play Random Clip</button>
<audio id="player" controls></audio>

<script>
async function playRandom() {
  const api = "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main";
  const res = await fetch(api);
  if (!res.ok) { alert("Failed to list audios"); return; }
  const files = await res.json();
  if (!files.length) { alert("No audios found"); return; }
  const pick = files[Math.floor(Math.random() * files.length)];
  const audio = document.getElementById("player");
  audio.src = pick.download_url;
  audio.play().catch(() => {/* user gesture may be required */});
}

document.getElementById("play").addEventListener("click", playRandom);
</script>


---

3) Fetch a random audio and save it — curl + jq + shuf (Linux / Termux)

# Requires: curl, jq, shuf, xargs (shuf available in coreutils)
curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | jq -r '.[].download_url' \
  | shuf -n 1 \
  | xargs -I{} curl -L "{}" -o random-audio.mp3

echo "Saved as random-audio.mp3"


---

4) Curl-only (no shuf) — using Python to pick random (works on systems with python)

curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | python3 -c "import sys, json, random; data=json.load(sys.stdin); print(random.choice(data)['download_url'])" \
  | xargs -I{} curl -L "{}" -o random-audio.mp3


---

5) Minimal one-liner (just print random URL)

curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | jq -r '.[].download_url' | shuf -n1


---

6) Example: Play streamed audio in terminal (mpv / mplayer)

# Print a random URL and stream it immediately with mpv
URL=$(curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
      | jq -r '.[].download_url' | shuf -n1)
mpv --no-video "$URL"

# or with mplayer
# mplayer "$URL"


---

Troubleshooting & tips

If you hit GitHub rate limits: authenticate requests by adding Authorization: token YOUR_TOKEN to fetch calls or use a personal access token in headers.

If there are no files in Audios/, the examples will show an error — just upload files into the Audios folder and try again.

If files have unusual extensions, the download_url will still point to the raw file; audio players that support that extension will play it.



---

Use cases

Bots (Discord, WhatsApp, Telegram): fetch random clip on command.

Frontend: lightweight background sounds or previews.

Testing: quick audio fixtures for UI/UX.



---

Credits

Created and maintained by 𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧
Repo: https://github.com/xhclintohn/Music-Clips-Collection


---