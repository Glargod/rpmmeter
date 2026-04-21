Here's a clean, well-structured **README.md** for your Engine Sound → RPM WebApp:

---

# Engine Sound → Perceived RPM WebApp

A simple browser-based tool that estimates engine RPM by analyzing exhaust sound through the device's microphone.

Perfect for quick diagnostics when the physical tachometer is broken or stuck.

## Features

- Real-time RPM estimation from engine sound
- 1-second update rate for stability
- Adjustable calibration multiplier
- Low-pass filter option (in previous versions)
- Waveform visualizer
- Works on desktop and mobile (iOS + Android)
- No server required — runs entirely in the browser

## How It Works (Pseudocode)

```pseudocode
INITIALIZE:
    Create AudioContext
    Request microphone access with iOS-friendly constraints:
        echoCancellation = false
        noiseSuppression = false
        autoGainControl = false

    Connect: Mic → Analyser Node (fftSize = 2048)

MAIN LOOP (every 1 second):
    IF sound level < MIN_THRESHOLD (currently 8):
        Display RPM = 0
        Continue

    ELSE:
        Get raw audio waveform (time domain data)

        FIND FUNDAMENTAL FREQUENCY:
            For offset from 35 to (sampleRate / 120 Hz):
                Calculate autocorrelation score
                Keep the offset with the highest score

            frequency = sampleRate / best_offset

            IF frequency > 120 Hz:
                frequency = 120 Hz   // hard cap

        CALCULATE RPM:
            multiplier = (is4Stroke ? cylinders / 2 : cylinders)
            rpm = frequency * 60 / multiplier * calibration_multiplier

        Display RPM
        Update waveform visualization
```

### Key Parameters

- **MIN_LEVEL_THRESHOLD**: 8 (minimum sound energy to process)
- **MAX_FREQ**: 120 Hz (prevents detecting high harmonics as fundamental)
- **fftSize**: 2048 (good balance between resolution and performance)
- **Update Rate**: 1000 ms (1 second)
- **Calibration**: Default 0.10 (adjust based on your engine)

## How to Use

1. Open the HTML file in a modern browser (Safari on iOS recommended)
2. Click **"Start Mic + RPM"**
3. Allow microphone permission when prompted
4. Hold the bottom of your phone **very close** to the exhaust tip
5. Adjust the **Calibration** slider until the displayed RPM matches your real tachometer at idle
6. Rev the engine and observe the reading

## Calibration Tips

- Start at **0.10**
- For most 4-stroke single-cylinder or small engines, the value usually lands between **0.08 – 0.18**
- Use a known RPM (idle or 400 Hz test tone) to fine-tune

## Limitations

- Performance depends heavily on phone microphone quality
- iOS Safari applies heavy processing — results can vary
- Very low idle frequencies (<25 Hz) can be hard to detect reliably
- Background noise and wind can affect accuracy
- Best used as a rough reference, not a precise instrument

## Future Improvements

- Multiple harmonic tracking
- Adaptive threshold
- Better peak detection algorithm
- Save calibration presets per engine
- Export recording + RPM log

---

Would you like me to also add:

- Installation / Hosting instructions?
- Troubleshooting section (especially for iOS)?
- A "How to Calibrate" guide with examples?

Just say what you want included and I’ll expand the README.
