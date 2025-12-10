# 🎵 Music Clips Collection

A **simple** repository containing very short music audio clips (≤ 30 seconds) stored in the `Audios/` folder.  
This README provides **copy-pasteable code examples** to fetch and use a **random audio file** directly from the repository — no need to know filenames!

> **Repo**: `xhclintohn/Music-Clips-Collection`  
> **Folder**: `Audios/`  
> **Creator**: **𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧**

---

[![Follow Developer](https://img.shields.io/github/followers/xhclintohn?label=Follow%20Me ;)&style=for-the-badge)](https://github.com/xhclintohn)

---

## How It Works

1. Use the GitHub Contents API to list all files in the `Audios/` folder:  
   `GET https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main`

2. Randomly select one file from the returned JSON array.

3. Use the `download_url` field to directly stream or download the raw audio file.

---

## ✅ Important Notes

- No API key required for public repositories (subject to GitHub's unauthenticated rate limits).
- All examples are minimal and designed to be copy-paste friendly.
- Supported formats: `.mp3`, `.ogg`, `.m4a`, `.wav`, etc. — any audio file works via raw `download_url`.

---

## Examples

### 1. Fetch Random Audio URL — Node.js (Recommended)

```js
// Save as fetch-random-audio.js (Node.js 18+ has built-in fetch)
async function getRandomAudio() {
  const api = "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main";
  const res = await fetch(api);
  if (!res.ok) throw new Error(`GitHub API error: ${res.status}`);
  
  const files = await res.json();
  if (!Array.isArray(files) || files.length === 0) throw new Error("No audio files found.");
  
  const randomFile = files[Math.floor(Math.random() * files.length)];
  return randomFile.download_url; // Direct link to raw audio
}

(async () => {
  try {
    const url = await getRandomAudio();
    console.log("Random audio URL:", url);
    
    // Optional: Download the file
    // const audioRes = await fetch(url);
    // const buffer = await audioRes.arrayBuffer();
    // require('fs').writeFileSync('random-clip.mp3', Buffer.from(buffer));
  } catch (e) {
    console.error("Error:", e.message);
  }
})();
2. Play Random Clip in Browser (Vanilla JS)
<!DOCTYPE html>
<html>
<head>
  <title>Random Music Clip Player</title>
</head>
<body>
  <button id="play">Play Random Clip</button>
  <audio id="player" controls></audio>

  <script>
    async function playRandom() {
      const api = "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main";
      try {
        const res = await fetch(api);
        if (!res.ok) throw new Error("Failed to fetch file list");
        
        const files = await res.json();
        if (files.length === 0) throw new Error("No audio files found");
        
        const randomFile = files[Math.floor(Math.random() * files.length)];
        const audio = document.getElementById("player");
        audio.src = randomFile.download_url;
        audio.play().catch(err => console.log("Playback prevented (user gesture needed)"));
      } catch (err) {
        alert("Error: " + err.message);
      }
    }

    document.getElementById("play").addEventListener("click", playRandom);
  </script>
</body>
</html>
3. Download Random Clip — Bash (curl + jq + shuf)
# Requires: curl, jq, shuf (Linux/macOS/Termux)
curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | jq -r '.[].download_url' \
  | shuf -n 1 \
  | xargs -I {} curl -L "{}" -o random-clip.mp3

echo "Downloaded as random-clip.mp3"
4. Curl + Python One-liner (No shuf needed)
curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | python3 -c "import sys, json, random; data=json.load(sys.stdin); print(random.choice(data)['download_url'])" \
  | xargs -I {} curl -L "{}" -o random-clip.mp3
5. Just Print a Random URL (One-liner)
curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
  | jq -r '.[].download_url' | shuf -n 1
6. Stream Random Clip in Terminal (mpv or mplayer)
# Using mpv
URL=$(curl -s "https://api.github.com/repos/xhclintohn/Music-Clips-Collection/contents/Audios?ref=main" \
      | jq -r '.[].download_url' | shuf -n 1)
mpv --no-video "$URL"

# Or with mplayer
# mplayer "$URL"
Troubleshooting & Tips
Rate limited? Authenticate with a GitHub token:
Add header: -H "Authorization: token YOUR_TOKEN"
No files found? Make sure audio files are uploaded to the Audios/ folder.
Unusual file extensions? Raw URLs still work — players will handle supported formats.
Use Cases
Discord/Telegram/WhatsApp bots (play random clips on command)
Web apps (background music, sound effects)
UI/UX testing (quick audio samples)
Fun projects and memes!
Credits
Created and maintained by 𝐱𝐡_𝐜𝐥𝐢𝐧𝐭𝐨𝐧
Repository: https://github.com/xhclintohn/Music-Clips-Collection
Enjoy the clips! 🎧 Feel free to contribute more short audio files!
This version is clean, professional, well-structured, and ready to paste directly into your GitHub repository's README.md. It will render beautifully on GitHub! 🚀