# ASK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Theory
## ASK
Amplitude Shift Keying (ASK) is a digital modulation technique in which the amplitude of a carrier signal is varied according to the digital data (binary 0 and 1) while the frequency and phase remain constant.
* Binary 1 → Carrier signal with high amplitude
* Binary 0 → Carrier signal with low or no amplitude
## FSK
Frequency Shift Keying (FSK) is a digital modulation technique in which the frequency of the carrier signal is changed according to the digital input data (binary 0 and 1) while the amplitude remains constant.
* Binary 1 → Higher frequency (f₁)
* Binary 0 → Lower frequency (f₀)
# Tools required
PC & Google Colab
# Program
## ASK
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs), 'low')
    return lfilter(b, a, x)

# Parameters
fs, fc, br, T = 1000, 50, 10, 1
t = np.arange(0, T, 1/fs)

# Message signal
bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, fs//br)

# Carrier signal
carrier = np.sin(2*np.pi*fc*t)

# ASK modulation & demodulation
ask = msg * carrier
demod = lpf(ask * carrier, fc, fs)
decoded = (demod[::fs//br] > 0.25).astype(int)

# Plot
plt.figure(figsize=(10,9))
 

plt.subplot(4,1,1)
plt.plot(t, msg)
plt.title("Message Signal")

plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")

plt.subplot(4,1,3)
plt.plot(t, ask)
plt.title("ASK Modulated Signal")

plt.subplot(4,1,4)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Decoded Bits")

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

```
## FSK

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low-pass filter
def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs), 'low')
    return lfilter(b, a, x)

# Parameters
fs, f1, f2, br, T = 1000, 30, 70, 10, 1
t = np.arange(0, T, 1/fs)
bd = fs // br

# Message signal
bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, bd)

# Carrier signals
c1 = np.sin(2*np.pi*f1*t)
c2 = np.sin(2*np.pi*f2*t)

# FSK Modulation
fsk = np.zeros_like(t)
for i, b in enumerate(bits):
    fsk[i*bd:(i+1)*bd] = np.sin(2*np.pi*(f2 if b else f1)*t[i*bd:(i+1)*bd])

# Demodulation (correlation)
d1 = lpf(fsk * c1, f1, fs)
d2 = lpf(fsk * c2, f2, fs)

dec = [(np.sum(d2[i*bd:(i+1)*bd]**2) >
        np.sum(d1[i*bd:(i+1)*bd]**2)) for i in range(br)]
demod = np.repeat(dec, bd)

# Plot
plt.figure(figsize=(10,10))


plt.subplot(5,1,1); plt.plot(t, msg); plt.title("Message Signal")
plt.subplot(5,1,2); plt.plot(t, c1); plt.title("Carrier f1 (bit 0)")
plt.subplot(5,1,3); plt.plot(t, c2); plt.title("Carrier f2 (bit 1)")
plt.subplot(5,1,4); plt.plot(t, fsk); plt.title("FSK Modulated Signal")
plt.subplot(5,1,5); plt.plot(t, demod); plt.title("Demodulated Signal")

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()

```
# Output Waveform
## ASK
<img width="733" height="419" alt="Screenshot 2026-03-13 133033" src="https://github.com/user-attachments/assets/2975c3ea-cf26-4174-87f5-9fa0589003cd" />

## FSK
<img width="753" height="487" alt="Screenshot 2026-03-13 133115" src="https://github.com/user-attachments/assets/1f95cd10-1bea-4132-b7e4-8a5e1ea0c0f3" />

# Results
Thus the ASK  &  FSK are simulated using Python Program in Google Colab
