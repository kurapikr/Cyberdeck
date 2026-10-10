<div align="center">

# 🎧 Pocket Cyberdeck

### Your Spotify on a desk screen, plus an animated mini display.

<!-- 📸 PHOTO: hero shot of the finished cyberdeck on its stand, Spotify screen playing and OLED animating -->
<img src="images/hero.jpg" alt="Pocket Cyberdeck" width="700">

`ESP32` · `CYD` · `OLED` · `Spotify` · `Arduino IDE`

</div>

---

## What is this?

A fun portable cyberdeck. Not a formal tutorial, just how I built it.

- 🟢 **Big screen** shows what's playing on your Spotify. Tap to play, pause, skip, shuffle, change volume, and pick playlists.
- 🔵 **Small screen** plays any image, GIF, or video you choose.
- 🔋 **Battery powered.** Use it anywhere, or leave it on your desk as a display.

<!-- 📸 PHOTO: close-up of the Spotify screen -->
<!-- 📸 PHOTO: close-up of the OLED animation (a GIF works great here) -->
| Spotify screen | OLED animation |
|:---:|:---:|
| <img src="images/spotify-screen.jpg" width="340"> | <img src="images/oled.gif" width="340"> |

---

## Contents

1. [Hardware](#-1-hardware)
2. [Wiring](#-2-wiring)
3. [Software](#-3-software)
4. [Final touches](#-4-final-touches)

---

# 🧰 1. Hardware

### What you need

- 4 × PCB boards
- 1 × CYD (Cheap Yellow Display)
- 1 × ESP32 module
- 1 × OLED display (4 pins)
- 1 × Charging module with USB
- 1 × Lithium battery (1300 mAh)
- Wire
- Foam tape
- Phone stand

<!-- 🔗 LINKS: add a link to each part above if you want -->

### Also needed

- A computer
- A USB cable that can **transfer data** (not just charge)
- A Spotify **Premium** account
- A **2.4GHz** WiFi network

<!-- 📸 PHOTO: all the parts laid out together -->
<img src="images/parts.jpg" alt="All parts" width="600">

---

### Step 1 · Join the boards

Lay the 4 PCBs side by side to make one big board. Tie them together with wire through the holes.

> [!TIP]
> Wrap the wire however you like. I tangled mine because I liked how it looked. Just make sure it's stable.

<!-- 📸 PHOTO: the 4 PCBs tied together -->
<img src="images/step1-boards.jpg" alt="Joined boards" width="500">

### Step 2 · Attach the parts

Attach each part to the board by wrapping wire through the holes:

1. CYD
2. ESP32
3. OLED
4. Charging module
5. Battery

Place them so the ESP32 and OLED can be wired from the **back** of the board.

<!-- 📸 PHOTO: all parts attached on the front -->
<img src="images/step2-parts.jpg" alt="Parts attached" width="500">

---

# ⚡ 2. Wiring

Three small jobs: the battery, the CYD, and the ESP32 + OLED.

Here's how the power flows:

```
Battery ──► Charging module ──┬──► CYD
                              └──► ESP32 ──► OLED
```

> [!WARNING]
> Always check **+** and **−** before connecting anything. Wrong polarity can damage your parts.

<!-- 📸 PHOTO or DIAGRAM: full wiring overview from the back of the board -->
<img src="images/wiring-overview.jpg" alt="Wiring overview" width="600">

---

### Wiring A · The battery

Your charging module has four connections: **B+**, **B−**, **OUT+**, **OUT−**.

| Battery | Charging module |
|:---:|:---:|
| + | **B+** |
| − | **B−** |

<!-- 📸 PHOTO: close-up of the battery wires on B+ and B− -->
<img src="images/wiring-battery.jpg" alt="Battery wiring" width="500">

---

### Wiring B · The CYD

The CYD has a **BAT** port. Its cable comes with a plastic plug and 4 wires. You only need **2**.

1. Plug the cable into the **BAT** port.
2. Cut the cable so you can reach the charging module.
3. Use the wires that match the **+** and **−** printed next to the BAT port.

| CYD (BAT) | Charging module |
|:---:|:---:|
| + | **OUT+** |
| − | **OUT−** |

<!-- 📸 PHOTO: the CYD BAT port with the cable plugged in -->
<img src="images/wiring-cyd.jpg" alt="CYD wiring" width="500">

---

### Wiring C · The ESP32 and the OLED

**Power the ESP32:**

| ESP32 | Charging module |
|:---:|:---:|
| Vin | **OUT+** |
| GND | **OUT−** |

**Connect the OLED to the ESP32:**

| OLED | ESP32 |
|:---:|:---:|
| VCC | 3V3 |
| GND | GND |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

> [!TIP]
> The CYD and the ESP32 share the same **OUT+** and **OUT−**. Twist their wires together on each one.

<!-- 📸 PHOTO: close-up of the ESP32 and OLED wiring from the back -->
<img src="images/wiring-esp32-oled.jpg" alt="ESP32 and OLED wiring" width="500">

---

# 💻 3. Software

Two screens, two separate programs. Do the setup once, then follow Part A and Part B.

---

## Setup (do this once)

### Step 1 · Install Arduino IDE

Download it free from [arduino.cc/en/software](https://www.arduino.cc/en/software) and install it like any normal app. Open it once so it finishes setting up.

### Step 2 · Teach it about your board

1. Open **Arduino IDE → Settings**
2. Find **Additional Boards Manager URLs** and paste this in:

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

3. Go to **Tools → Board → Boards Manager**, search `esp32`, and install the one by **Espressif Systems**

---

## 🟢 Part A · Spotify screen (CYD)

I followed this great tutorial for this part. Here's the simple version.

> 🔗 **Original tutorial:** [PASTE TUTORIAL LINK](LINK_HERE)

### Step 1 · Plug in and connect

1. Plug the CYD into your computer
2. **Tools → Board** → choose **ESP32 Dev Module**
3. **Tools → Port** → choose the one that appears
4. **Tools → Upload Speed** → set to `115200`

> [!TIP]
> No port showing up? Your cable is probably charge-only. Swap it for one that can transfer files.

### Step 2 · Install the libraries

Go to **Sketch → Include Library → Manage Libraries** and install each of these by name:

`TFT_eSPI` · `XPT2046_Touchscreen` · `ArduinoJson` · `TJpg_Decoder` · `WiFiManager`

One library isn't in that search, so download it directly:

[⬇ Download SpotifyArduino (.zip)](https://github.com/witnessmenow/spotify-api-arduino/archive/refs/heads/main.zip)

Then in Arduino: **Sketch → Include Library → Add .ZIP Library** → select the file you just downloaded.

### Step 3 · Set up the display driver

Open this file on your computer:

```
Documents/Arduino/libraries/TFT_eSPI/User_Setup.h
```

Delete everything inside it and paste this in its place:

```cpp
#define ILI9341_DRIVER

#define TFT_MISO 12
#define TFT_MOSI 13
#define TFT_SCLK 14
#define TFT_CS   15
#define TFT_DC   2
#define TFT_RST  -1

#define TOUCH_CS -1

#define LOAD_GLCD
#define LOAD_FONT2
#define LOAD_FONT4
#define LOAD_FONT6
#define LOAD_FONT7
#define LOAD_FONT8
#define LOAD_GFXFF
#define SMOOTH_FONT

#define SPI_FREQUENCY  40000000
#define SPI_READ_FREQUENCY  20000000
#define SPI_TOUCH_FREQUENCY  2500000
```

Save the file.

### Step 4 · Create your Spotify app

1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard) and log in
2. Click **Create app** and name it anything
3. For **Redirect URI**, type exactly:

```
http://127.0.0.1:8888/callback
```

4. Check the **Web API** box, then Save
5. Copy your **Client ID**, then click **View client secret** and copy the **Client Secret** too

### Step 5 · Get your permanent login pass

Build this link (swap in your real Client ID), open it in a browser, and click **Agree**:

```
https://accounts.spotify.com/authorize?client_id=YOUR_CLIENT_ID&response_type=code&redirect_uri=http://127.0.0.1:8888/callback&scope=user-read-currently-playing%20user-read-playback-state%20user-modify-playback-state
```

The page will look "broken" after. That's expected. Copy everything after `code=` in the address bar.

Then in Terminal, run this (fill in your own values):

```bash
curl -X POST https://accounts.spotify.com/api/token \
-H "Content-Type: application/x-www-form-urlencoded" \
-d grant_type=authorization_code \
-d code=YOUR_CODE_HERE \
-d redirect_uri=http://127.0.0.1:8888/callback \
-d client_id=YOUR_CLIENT_ID \
-d client_secret=YOUR_CLIENT_SECRET
```

Copy the `refresh_token` from the response. That's your permanent pass.

> [!WARNING]
> The code from the browser step expires in about 10 minutes. Do both parts of this step back to back.

### Step 6 · Get the code and add your keys

Copy the full code from the tutorial and paste it into a new Arduino sketch.

> 🔗 **Code:** [PASTE CODE LINK](LINK_HERE)

Near the top, fill in your three values:

```cpp
char clientId[] = "paste it here";
char clientSecret[] = "paste it here";
char refreshToken[] = "paste it here";
```

> [!CAUTION]
> Never upload your real keys to GitHub. Keep them on your computer only.

<!-- 📸 PHOTO or SCREENSHOT: Arduino IDE with the sketch open -->
<img src="images/partA-arduino.png" alt="Arduino sketch" width="600">

### Step 7 · Upload to the CYD

Click **Upload**. Watch the bottom panel for "Compiling" then "Writing."

> [!TIP]
> Stuck on "Connecting..."? Hold the **BOOT** button on the board until it starts writing, then let go.

### Step 8 · Connect it to WiFi

1. On first boot, the CYD creates its own network called `Spotify-CYD`
2. Connect your phone to it. A setup page should pop up
3. Pick your home WiFi and type the password there

> [!WARNING]
> Use your **2.4GHz** network. This board can't connect to 5GHz WiFi.

### Step 9 · Play something

Start playing a song on Spotify. The screen shows what's playing. Tap to play, pause, skip, shuffle, or change the volume.

> [!NOTE]
> The screen mirrors your playback. It doesn't play the audio itself.

<!-- 📸 PHOTO: Spotify screen showing a song -->
<img src="images/partA-result.jpg" alt="Spotify screen working" width="500">

### If you get stuck

| Problem | Fix |
|---|---|
| Upload fails or hangs | Try a different data cable, or hold BOOT while it connects |
| Touch feels backwards | Swap the min/max touch calibration numbers to flip that axis |
| Nothing shows when playing music | Make sure Spotify is actively playing somewhere |
| `SpotifyArduino.h` not found | Add it with the ZIP download, not the Library Manager search |

---

## 🔵 Part B · Animated OLED (ESP32)

This screen plays any image, GIF, or video you want. A free website turns it into code for you.

🌐 [oledanimationmaker.com](https://www.oledanimationmaker.com/)

### Step 1 · Add your media

1. Open [oledanimationmaker.com](https://www.oledanimationmaker.com/)
2. Import the image, GIF, or video you want to show
3. Check the preview

<!-- 📸 PHOTO or SCREENSHOT: the website with your animation loaded -->
<img src="images/partB-website.png" alt="OLED Animation Maker" width="600">

### Step 2 · Copy the code

When you like the preview, copy the code the website gives you.

### Step 3 · Plug in and connect

1. Plug the ESP32 into your computer
2. Open a new sketch in Arduino and paste the code
3. **Tools → Board** → choose **ESP32 Dev Module**
4. **Tools → Port** → choose the one that appears

> [!NOTE]
> No extra libraries needed for this one.

### Step 4 · Upload to the ESP32

Click **Upload**. Watch the bottom panel for "Compiling" then "Writing."

> [!TIP]
> Stuck on "Connecting..."? Hold the **BOOT** button on the board until it starts writing, then let go.

### Step 5 · Watch it play

Your animation should start looping on the OLED.

> [!TIP]
> Blank screen? Check that **SDA** is on GPIO 21, **SCL** is on GPIO 22, and VCC and GND are on the right pins.

<!-- 📸 PHOTO or GIF: OLED playing the animation -->
<img src="images/partB-result.gif" alt="OLED animation working" width="500">

---

# ✨ 4. Final touches

### Step 1 · Test everything

Turn it on with the battery. Check that the Spotify screen connects and the OLED animation plays. Make sure all the wiring is stable.

### Step 2 · Add foam tape

Once everything works, stick foam tape on the back of the board. This keeps it stable and comfortable to carry.

<!-- 📸 PHOTO: the back of the board with foam tape -->
<img src="images/final-back.jpg" alt="Foam tape on the back" width="500">

### Step 3 · Add a stand

Place it on a phone stand to display it on your desk. You can buy one or 3D print one.

<!-- 📸 PHOTO: the cyberdeck on its stand -->
<img src="images/final-stand.jpg" alt="Cyberdeck on a stand" width="600">

---

<div align="center">

## 🎉 That's it. You built it!

Post your build and tag me. I'd love to see yours. 👀

</div>

---

## Credits

- Spotify screen tutorial: [PASTE TUTORIAL LINK](LINK_HERE)
- [SpotifyArduino library](https://github.com/witnessmenow/spotify-api-arduino) by Brian Lough
- [OLED Animation Maker](https://www.oledanimationmaker.com/)
