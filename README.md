# Apollo CAST-1

<p align="center">
  <img src="CAST_Card.jpg" alt="Apollo CAST-1" width="600">
</p>

Easily make your speakers smart and cast to multiple devices using Music Assistant and Sendspin!

## Connectivity

One firmware covers both WiFi and Ethernet — there is no need to pick a
variant or reflash to switch:

- **Ethernet plugged in:** traffic moves to Ethernet. If the device is also
  joined to your WiFi, that connection stays up alongside, so Home Assistant
  never loses the device when you plug or unplug the cable. If WiFi has no
  network to join, it turns off entirely — the setup hotspot is never active
  while Ethernet is connected.
- **No Ethernet:** WiFi works as usual. The device rejoins its saved
  network, or opens the "Apollo CAST 1 Hotspot" for setup if none is
  configured (Improv via Bluetooth or USB works too).

Use the on-device **Network Connection** sensor (Diagnostics) to see which
connection is active at a glance.

## WizMote

The CAST-1 gives you full Home Assistant control over a paired Wiz WizMote. Each of the nine buttons (On, Off, Night, Brightness Up/Down, and 1–4) is a configurable select on the device — assign it Play, Pause, Play / Pause, Next/Previous Track, Volume Up/Down, Toggle Light, or Send HA Event. The defaults give you media controls out of the box.

Set any button to **Send HA Event** to trigger a custom Home Assistant action — play a Music Assistant playlist, toggle a room's lights, run a scene or script.

Configure every button without leaving Home Assistant using the [CAST-1 WizMote blueprint](https://github.com/ApolloAutomation/Blueprints/tree/main/CAST-1):

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FApolloAutomation%2FBlueprints%2Fblob%2Fmain%2FCAST-1%2FCAST-1-WizMote.yaml)

## Announcements

While an announcement plays, music ducks and the onboard LED lights up. Two sliders in Home Assistant set how much:

- **Announcement Ducking** (−80 to 0 dB, default −40) is how far the music drops. 0 turns ducking off.
- **Announcement LED Brightness** (0–100%, default 100%) is how bright the onboard LED is. 0 keeps the LED off.

## External LED Strip

The 5V / GND / IO1 header drives an addressable LED strip (WS2812, SK6812, WS2811, APA106 or SM16703). The firmware supports it out of the box, but its entities are disabled by default, since most CAST-1s don't have a strip connected. To use one, open the CAST-1's device page in Home Assistant and enable the **LED Strip** entities.

The strip's settings live in Home Assistant and under **External LED Strip** on the device's web page:

- **Count** is how many LEDs the strip has (up to 300, default 30).
- **Chipset**, **Color Order** and **Max Brightness** describe your strip.
- **Effect A** and **Effect B** are two effect slots. Each has a **Pattern** and a **Speed** (1 = one step per second, 100 = one step every 32 ms, on a log curve). Changes apply live while the effect runs. The patterns:
  - **Flicker** dips to random levels between full and 30% brightness.
  - **Strobe** is on for one step and off for the next.
  - **Rainbow** moves a rainbow along the strip. It ignores the light's color.
  - **Color Wipe** fills the strip one LED at a time, then clears it the same way.
  - **Scan** moves one lit LED back and forth.
  - **Twinkle** fades random LEDs in and out.
  - **Pulse** fades the whole strip from off to full and back.
  - **Fireworks** lights random sparks that spread and fade.

In automations, set the pattern and speed with `select.select_option` and `number.set_value`, then call `light.turn_on` with `effect: "Effect A"` (add `transition: 0` for an instant start).

> [!WARNING]
> **Photosensitivity:** Strobe flashes faster than 3 times per second from about Speed 50 up, and reaches about 16 flashes per second at Speed 100. Pulse passes 3 per second from about Speed 75. Flashing in this range can trigger seizures in people with photosensitive epilepsy.

Power long strips from their own 5V supply with a shared ground; the header can't supply a full-brightness strip, and **LED Strip Max Brightness** (default 50%) caps the draw.

Links: \
Discord (Support/feedback/discussion/future products): [http://dsc.gg/ApolloAutomation](http://dsc.gg/ApolloAutomation) \
Shop: [https://apolloautomation.com](https://apolloautomation.com) \
Wiki: [https://wiki.apolloautomation.com](https://wiki.apolloautomation.com/)
