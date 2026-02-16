# Pulse-Code-Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
 COLAB
# Program
```
1. PULSE CODE MODULATION:

import numpy as np
import matplotlib.pyplot as plt

fs, f, T, L = 5000, 50, 0.1, 16
t = np.linspace(0, T, int(fs*T), False)

msg = np.sin(2*np.pi*f*t)
clk = np.sign(np.sin(2*np.pi*200*t))

step = (msg.max()-msg.min())/L
q_msg = np.round(msg/step)*step

plt.figure(figsize=(10,8))

plt.subplot(4,1,1); plt.plot(t,msg); plt.title("Analog Message"); plt.grid()
plt.subplot(4,1,2); plt.plot(t,clk); plt.title("Clock Signal"); plt.grid()
plt.subplot(4,1,3); plt.step(t,q_msg); plt.title("PCM Signal"); plt.grid()
plt.subplot(4,1,4); plt.plot(t,q_msg,'--'); plt.title("PCM Demodulation"); plt.grid()

plt.tight_layout()
plt.show()
```
```
2. DELTA MODULATION:

import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

fs, f, T, d = 10000, 10, 1, 0.1
t = np.arange(0, T, 1/fs)
msg = np.sin(2*np.pi*f*t)

dm, bits = [0], []
for s in msg:
    step = d if s > dm[-1] else -d
    bits.append(step > 0)
    dm.append(dm[-1] + step)

demod = np.cumsum([0] + [d if b else -d for b in bits])
b, a = butter(4, 20/(0.5*fs))
filt = filtfilt(b, a, demod)

plt.figure(figsize=(10,6))
plt.subplot(311); plt.plot(t,msg); plt.title("Original"); plt.grid()
plt.subplot(312); plt.step(t,dm[:-1],where='mid'); plt.title("DM Signal"); plt.grid()
plt.subplot(313); plt.plot(t,filt[:-1],'r:'); plt.title("Demodulated"); plt.grid()

plt.tight_layout()
plt.show()
```
# Output Waveform
```
1. PULSE CODE MODULATION:
```
<img width="990" height="790" alt="image" src="https://github.com/user-attachments/assets/d16d2b60-ae77-4e72-b3a9-7b744f8e7864" />

```
2. DELTA MODULATION:
```
<img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/304323d4-b482-475d-af89-cd807834e80f" />

# Results:
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles.

