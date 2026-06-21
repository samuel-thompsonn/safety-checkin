# Safety Check-in

A two-page browser app for real-time safety monitoring. One person taps a button periodically to check in; if they miss it, an alarm blares on a friend's screen.

## Use case

You're doing something alone (a late-night walk, a solo activity) and a friend wants to be alerted if you stop responding. Your friend opens the monitor page on their laptop and goes to sleep. You keep the check-in page open and tap the button every few minutes. If you miss a check-in, the monitor page wakes your friend with a loud alarm.

## Files

- **`my-checkin.html`** — your page. Set the check-in interval and warning threshold, then tap the big button to check in before time runs out.
- **`friend-monitor.html`** — your friend's page. Polls Firebase every 3 seconds and mirrors your countdown. Plays a looping Web Audio alarm if you miss a check-in.

## How to use

1. Open `friend-monitor.html` on your friend's device (keep volume up).
2. Open `my-checkin.html` on your device.
3. Configure the interval and click **Start session**.
4. Tap **Check in** before the timer runs out. Repeat until you're safe.

Both files work as local `file://` pages — no server needed.

## How it works

- The two pages communicate via [Firebase Realtime Database](https://firebase.google.com/docs/database) using plain `fetch` REST calls (no SDK).
- The checker writes a JSON event `{ type, ts, totalSec, warnSec }` on `start`, `checkin`, `warning`, `alert`, and `stop`.
- The monitor polls the same endpoint every 3 seconds and updates its display accordingly.
- The alarm is generated with the Web Audio API — no audio files required.

## Firebase

The app uses a Firebase Realtime Database at:
```
https://sam-check-in-default-rtdb.firebaseio.com/session.json
```

To use your own Firebase project, replace the `DB` constant at the top of each file's `<script>` block. The database rules for personal use can be open read/write:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

## No build tools

Plain HTML/CSS/JS. No npm, no framework, no bundler.
