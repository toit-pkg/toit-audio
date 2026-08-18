# Audio

Packed PCM processing and allocation-conscious DSP operations for Toit.

The package provides:

- packed PCM conversion, channel extraction, decimation, and saturating mixing;
- reductions and allocation-free framed energy extraction;
- normalized correlation and Goertzel frequency banks;
- reusable real and complex FFT plans using Q15 or float32 data;
- pointwise complex multiplication, magnitude, and power operations;
- GCC-PHAT delay estimation, FIR and biquad filters, and linear resampling.

The operations use caller-owned byte arrays and native SDK primitives. MP3 is
not included.

## Usage

```toit
import audio

main:
  samples := #[0, 0, 0xff, 0x7f]
  energy := audio.mean-absolute-energy samples
      --format=audio.PCM-S16-LE
  print energy
```

## Embedded firmware options

Audio primitives are disabled by default in embedded SDK builds. Enable the
tiers needed by the application in the firmware configuration:

- `TOIT_AUDIO` provides packed PCM operations, reductions, and framed energy.
- `TOIT_AUDIO_EXTRA` adds correlation, Goertzel, Q15 FFTs, GCC-PHAT, filters,
  resampling, and Q15 complex operations.
- `TOIT_AUDIO_FLOAT_FFT` adds float32 complex FFT and pointwise operations and
  depends on `TOIT_AUDIO_EXTRA`.

The Q15 variants use less memory and work well on targets without efficient
floating-point arithmetic. Float32 has simpler scaling and more dynamic range,
which is especially convenient for convolution and spectral processing.
