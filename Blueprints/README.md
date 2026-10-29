# Porch Light Sequence Blueprints

Home Assistant blueprints for a Halloween porch display. As a visitor walks up to the door, lights turn on in stages along the path. Each stage can play a sound, and the visitor reaching the door plays a finale. Afterwards the lights go to a resting state. If the visitor turns back partway, the lights go to the resting state on their own instead of staying on.

They work with any Home Assistant lights (a CAST-1 LED strip, Hue bulbs and so on) and any speaker. Pick the blueprint that matches your sensors:

**Zone-Based**: Presence zones, such as LD2450 radar zones made with Zone Mapper, or any on/off occupancy sensors

[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FApolloAutomation%2FCAST-1%2Fblob%2Fmain%2FBlueprints%2FCAST-1-Porch-Zone-Based.yaml)

**Distance-Based**: One radar distance reading, such as an LD2410 or LD2412 (Apollo R PRO-1) mounted by the door

[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FApolloAutomation%2FCAST-1%2Fblob%2Fmain%2FBlueprints%2FCAST-1-Porch-Distance-Based.yaml)

**Motion-Based**: One or more PIR motion sensors, such as the ESPHome Starter Kit

[![Import blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FApolloAutomation%2FCAST-1%2Fblob%2Fmain%2FBlueprints%2FCAST-1-Porch-Motion-Based.yaml)

All three need Home Assistant 2025.7 or newer.

## Install

1. Click **Import** next to the blueprint you want, then click **Preview** and **Import blueprint** in Home Assistant.
2. Go to **Settings → Automations & scenes → Blueprints**, select the blueprint, and fill it in. The steps below explain each section.
3. Save the automation.

To install by hand instead, copy the `.yaml` file into `config/blueprints/automation/apollo/` in Home Assistant. Then go to **Developer tools → YAML** and click **Automations** under reload.

## Set up the stages

Each stage is a group of lights that turns on as the visitor gets closer. Click **Add** under **Stages** for each one. Stages that are already on stay on as later ones light up.

- **Lights:** one or more lights per stage. With a CAST-1 strip, use [partition lights](../README.md#splitting-the-strip-into-segments) to give each stage its own section of the strip.
- **Look:** every stage uses the shared **Stage look** (color, brightness and effect) unless you set its own.
- **Effects:** pick an effect from the list or type the name exactly as your light shows it. Each light only gets effects it supports; other lights just change color and brightness.
- **Sounds:** the optional **Stage sound** plays as each stage turns on. A stage can also have its own sound.

> [!WARNING]
> Strobe-style effects faster than 3 flashes per second can trigger seizures in people with photosensitive epilepsy.

**Finale:** the finale switches every lit light, plus any **Extra finale lights** such as a porch light, to the finale look for the **Finale length**. Then the lights go to the **Resting state**. The resting state can turn them off, show a resting look, or put them back the way they were before the sequence.

**Turned back:** if the visitor doesn't reach the next stage within the time limit, the lit lights go to the resting state. The optional **Turned-back sound** plays then. In Motion-Based, this only applies to a stage or finale that waits for its own sensor; a show running on timers always plays through.

## Zone-Based

- Add one stage per zone, in the order a visitor walks through them, and set the **Finale zone** to the zone by the door. Zones can be any on/off occupancy, presence or motion sensor.
- If a visitor enters a later zone first, the stages in between turn on with it.
- Stages only move toward the door. The **Reverse walk guard** stops someone walking out of the house from starting a sequence.

## Distance-Based

- Mount the radar by the door, facing out, so the distance shrinks as a visitor walks up.
- **Distance sensor:** use the radar's **Moving Distance**, which ignores still objects such as plants or parked cars.
- **Target sensor:** use the radar's overall presence or target sensor, which stays on for people standing still.
- **Distances:** give each stage the distance at which it turns on, and set a smaller **Finale distance**. Use the unit your sensor shows in Home Assistant, such as inches or cm. Stages can be added in any order; they're sorted farthest first.
- **Spacing:** LD2410 radars measure in steps of about 75 cm (30 in), so space stages at least that far apart.
- **Starting:** a sequence starts when a visitor walks in across the first stage's distance. Keep that distance a little inside the radar's range so it sees visitors coming.

## Motion-Based

- **Start motion sensor:** motion here starts the sequence.
- **Each stage and the finale:** each either waits for its own motion sensor or turns on after a wait time.
  - **One sensor:** leave the stage and finale sensors empty, and the whole show runs on timers. The first stage turns on after the **First stage wait time** (0 s by default). Each later stage follows after the **Later stage wait time**, and the finale after the **Finale wait time**.
  - **More sensors:** give a stage or the finale its own sensor and it waits for that sensor instead. Use a different sensor for each.
- **Quiet time before a new show:** stops a visitor waiting at the door from replaying the show every time they move.
- **Reverse walk guard:** with more than one sensor, it stops someone leaving the house from starting a show. Set it to about how long the walk from the door to the start sensor takes.

## Sound and music

- **Sound player:** set it under **Sound player and options** to play every sound on one speaker. If you play music through Music Assistant, pick the speaker's own media player, not the Music Assistant one. Otherwise Music Assistant pauses the music for each sound instead of playing over it.
- **Ducking on a CAST-1:** music ducks under each sound, by 40 dB by default. Set `announcement_ducking_db` in your device YAML to keep the music louder (see [Announcements](../README.md#announcements)).
- **Light delay after a sound:** if your speaker starts sounds late, this makes the lights wait so they land together.
- **Volume:** **Set the volume during a sequence** changes the speaker's volume while a sequence runs, then puts it back.
- **CAST-1 Music Mode:** add the strip's **LED Strip Music Mode** switch to **Switches to pause during a sequence**. Otherwise music starting or stopping can take over the strip mid-show. It's turned back on afterwards.
- **Enable switch:** set one, such as a Halloween mode toggle, to turn the display on and off without editing the automation.

## Troubleshooting

If something doesn't happen as expected, open the automation, click **Traces** and select the run. The conditions step shows why a sequence didn't start, and each step shows what the lights and speaker were sent.
