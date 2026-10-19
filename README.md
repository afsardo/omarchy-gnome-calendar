# Omarchy GNOME Calendar Clock

Omarchy's bar clock, with your calendar events in its popup.

- **Dots** under each day with events, one per calendar, in that calendar's color.
- **Agenda**: click a day to list its events (time, title, calendar color) under the grid. The popup opens on today.
- **Open in Calendar**: jumps to GNOME Calendar on the selected day. Double-clicking a day does the same.

Everything else works like the stock `omarchy.clock`: label formats, right-click to cycle formats, middle-click for the timezone picker, and the year/life bars.

## How it works

Events are read from **Evolution Data Server**, the same data GNOME Calendar, Evolution and the GNOME reminder service use. Any account you add there works: Google, CalDAV, iCloud, Nextcloud, local calendars.

- Only calendars that are ticked in GNOME Calendar or Evolution are shown.
- Cancelled events and events you declined are skipped.
- A meeting that appears on two of your calendars is listed once.
- Events load in the background when the shell starts and every 5 minutes, covering two months back to three ahead, so the popup opens instantly.

## Requirements

- Omarchy with the Quickshell-based `omarchy-shell` and plugin support
- `evolution-data-server`, `python-gobject` and `gnome-calendar`
- At least one calendar account, added through Evolution (**File → New → Calendar**, or **Collection Account**)

```sh
omarchy pkg add evolution gnome-calendar python-gobject
```

## Install

```sh
omarchy plugin add https://github.com/afsardo/omarchy-gnome-calendar.git --enable
```

Then replace the stock clock in your bar: in `~/.config/omarchy/shell.json`, change `"id": "omarchy.clock"` to `"id": "afsardo.gnome-calendar"` and keep the other clock settings in that entry as they are.

After editing the plugin's files, run `omarchy restart shell`. The automatic plugin reload does not pick up changes to the popup.

## Tips

**Google calendars show a 405 error in Evolution.** If Evolution saved a Google calendar without its calendar ID, the error mentions `https://apidata.googleusercontent.com/caldav/v2/`. Close Evolution, then in that calendar's file under `~/.config/evolution/sources/`, set:

```ini
ResourcePath=/caldav/v2/<your-calendar-id>/events/
```

For your main calendar, the ID is your email address with `@` written as `%40`.

**Reminders with every app closed.** Start Evolution's reminder service on login, for example in `~/.config/hypr/autostart.lua`:

```lua
o.launch_on_start("/usr/lib/evolution-data-server/evolution-alarm-notify")
```

## Credits

Based on the `omarchy.clock` plugin from [Omarchy](https://github.com/basecamp/omarchy), MIT licensed. See `LICENSE-OMARCHY`.
