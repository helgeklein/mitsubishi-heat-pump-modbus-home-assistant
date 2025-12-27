# Home Assistant Configuration to Connect Mitsubishi Heat Pumps via Modbus

This configuration defines the most relevant Modbus registers of a Mitsubishi heat pump as Home Assistant entities. It's implemented as a HA package so that we can bundle Modbus entities with derived template sensors that make it easier to work with the data.

## Prerequisites

You need a Mitsubishi Modbus interface, either the A1M (serial only) or the A1M+ (serial + Ethernet). You can find details in this blog post.

## Setup

1. Locate the config directory of your HA instance (that's where the file `configuration.yaml` is stored).

2. Edit `configuration.yaml`, adding a packages directory as follows:

```YAML
# Load custom packages, e.g., for grouping raw sensors and derived templates
homeassistant:
  packages: !include_dir_named packages/
```

3. Create a folder `packages` in your HA config directory.

4. Copy the file `mitsubishi_a1m.yaml` from this repository to your `packages` folder.

5. Edit `mitsubishi_a1m.yaml`, filling in the hostname or IP address of your Modbus interface.

6. Restart HA.

You should now see a few dozen new entities in Home Assistant (try filtering the list for `mitsubishi`).

## Translations

### Entity Names

I would have like to provide localized entity names in English and German. Alas, that doesn't seem to be possible with Home Assistant Modbus entities. So I set up German entity names but provided the English translations as comments.

### Enum String Values

Several entities show the state of something as a number (an enum in developer lingo). To make it easier to work with those, I provided text representations for each state via template sensors. These strings *are* localized via a translation table stored in the sensor `Mitsubishi WP: Translations`. The input `Mitsubishi WP: Translations Language` makes it easy to switch between English and German for the enum strings.

## Renaming Entities

It's important to keep in mind that HA auto-generates entity IDs from the respective entity names. This is done by changing all text to lower-case and replacing any "weird" characters with underscores.

As our template sensors depend on specific entity IDs, use the following approach when renaming entities (e.g., when translating them to another language):

- Change the name.
- Deduce the new entity ID from your changed name.
- Search and replace the old entity ID (used as the unique ID, too) with the new entity ID.