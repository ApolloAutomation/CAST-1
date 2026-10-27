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

## External LED Strip

The 5V / GND / IO1 header drives an addressable LED strip (WS2812, SK6812, WS2811, APA106 or SM16703). Every setting lives in Home Assistant and under **External LED Strip** on the device's web page — no YAML edits needed.

- **LED Strip Count** and **LED Strip Segments** set how many LEDs you have and how many equal segments they split into (1–8). Leftover LEDs go to the last segment, so 22 LEDs in 3 segments are 7 / 7 / 8.
- **LED Strip Segment 1–8** switch each segment on or off. Switches past the Segments count do nothing.
- **Effect A** and **Effect B** are two effect slots on the LED Strip light. Each has a **Pattern** (Flicker or Strobe), a **Speed** (1 = one change per second, 100 = one every 30 ms, on a log curve) and an **Intensity** (Flicker: how deep it dips; Strobe: how much of each flash is lit). Changes apply live while the effect runs.
- The **Segments** effect shows a solid color on the segments that are on. Switching a segment off while the strip shows a plain color moves it onto this effect automatically.

In automations, set the pattern, speed and segments with `select.select_option`, `number.set_value` and `switch.turn_on` / `switch.turn_off`, then call `light.turn_on` with `effect: "Effect A"` (add `transition: 0` for an instant start).

> [!WARNING]
> **Photosensitivity:** Strobe flashes faster than 3 times per second from about Speed 50 up, and reaches about 16 flashes per second at Speed 100. Flashing in this range can trigger seizures in people with photosensitive epilepsy.

Power long strips from their own 5V supply with a shared ground; the header can't supply a full-brightness strip, and **LED Strip Max Brightness** (default 50%) caps the draw.

Links: \
Discord (Support/feedback/discussion/future products): [http://dsc.gg/ApolloAutomation](http://dsc.gg/ApolloAutomation) \
Shop: [https://apolloautomation.com](https://apolloautomation.com) \
Wiki: [https://wiki.apolloautomation.com](https://wiki.apolloautomation.com/)
