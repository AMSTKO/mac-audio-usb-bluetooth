# mac-audio-usb-bluetooth

Small macOS command-line script that keeps audio routing simple:

- input: first available USB audio input device
- output: first connected Bluetooth or Bluetooth LE audio output device

It uses CoreAudio directly, so it does not need Homebrew packages or GUI automation.

## Requirements

- macOS
- Swift command-line tools available at `/usr/bin/swift`
- A USB microphone/audio interface connected
- A Bluetooth speaker/headphones device already connected to macOS

## Usage

Run once:

```sh
./switch-audio-usb-bluetooth
```

List detected audio devices without changing anything:

```sh
./switch-audio-usb-bluetooth --list
```

Keep watching for audio changes and reapply the routing automatically:

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

- applies the routing on startup
- listens for CoreAudio device/default-device changes
- reapplies the routing when another app or macOS changes it
- waits without changing output if no Bluetooth output is connected

## Stop Watch Mode

Press `Ctrl-C` in the terminal running the script.

## Notes

The script only selects audio devices that macOS already sees. It does not connect Bluetooth devices by itself.
