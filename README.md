# mac-audio-usb-bluetooth

Small macOS command-line script that keeps audio routing predictable:

- input: first available USB audio input device
- output: first connected Bluetooth or Bluetooth LE audio output device

It uses CoreAudio directly. No Homebrew package, GUI scripting, or third-party
audio switcher is required.

## Requirements

- macOS
- Swift command-line tools available at `/usr/bin/swift`
- A USB microphone/audio interface connected
- A Bluetooth speaker/headphones device already connected to macOS

## Quick Start

List the audio devices macOS currently sees:

```sh
./switch-audio-usb-bluetooth --list
```

Apply the default routing once:

```sh
./switch-audio-usb-bluetooth
```

Keep watching for audio changes and reapply the routing automatically while the
command is running:

```sh
./switch-audio-usb-bluetooth --watch
```

The watch mode uses CoreAudio change notifications. It does not poll in a tight loop, so CPU usage should stay near zero while nothing changes.

## Choosing Specific Devices

If several USB inputs or Bluetooth outputs are connected, pass part of the device name:

```sh
./switch-audio-usb-bluetooth --input-name "Shure" --output-name "AirPods"
./switch-audio-usb-bluetooth --watch --input-name "Shure" --output-name "AirPods"
```

Name matching is case-insensitive. Exact device names and CoreAudio device UIDs also work.

## Behavior

Normal mode:

- finds a USB input and a Bluetooth/Bluetooth LE output
- sets macOS default input, default output, and system output
- exits with a clear error if either side is missing

Watch mode:

- applies the routing when the command starts
- listens for CoreAudio device/default-device changes
- reapplies the routing when another app or macOS changes it
- waits without changing output if no Bluetooth output is connected

## Stop Watch Mode

Press `Ctrl-C` in the terminal running the script.

## Notes

The script only selects audio devices that macOS already sees. It does not
connect Bluetooth devices by itself.

If the Bluetooth output is not listed, connect it in macOS first, then run
`--list` again.
