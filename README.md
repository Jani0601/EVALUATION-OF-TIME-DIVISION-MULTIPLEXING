# EVALUATION-OF-TIME-DIVISION-MULTIPLEXING

### Aim:
Study of TDM pulse amplitude modulation/ demodulation with transmitter block (clock) and channel identification information linked directly to the receivers.  

### Apparatus Required:

1)	Experimental kit DCL-02  
2)	Connecting chord  
3)	Power supply  
4)	20 MHz dual trace oscilloscope.  

### Theory:
In PAM, PPM the pulse is present for a short duration and for most of the time between the two pulses no signal is present. This free space between the pulses can be occupied by pulses from other channels. This is known as Time Division Multiplexing. Thus, time division multiplexing makes maximum utilization of the transmission channel. Each channel to be transmitted is passed through the low pass filter. The outputs of the low pass filters are connected to the rotating sampling switch (or) commutator.
It takes the sample from each channel per revolution and rotates at the rate of f s. Thus the sampling frequency becomes fs the single signal composed due to multiplexing of input channels. These channels signals are then passed through low pass reconstruction filters. If the highest signal frequency present in all the channels is fm, then by sampling theorem, the sampling frequency fs must be such that fs≥2fm. Therefore, the time space between successive samples from any one input will be  Ts=1/fs, and Ts =1/2fm.

### Procedure:

1)	Refer to the block diagram and carry out the following connections and switch setting. 
2)	Connect power supply in proper polarity DCL-02 and s it switch it on.  
3)	Connect 250hz, 500hz, 1khz and 2khz sine wave signals from the function generator to the multiplexer input channels CH0, CH1,CH3 by means of connecting chords.  
4)	Connect the multiplexer output txd of the transmitted section to the de multiplexer input rxd of the receiver section.  
5)	Connect the output of the receiver section CH0, CH1, CH2,CH3 to the IN0, IN1, IN2. IN3 of the filter section.  
6)	Connect the sampling clock TX CLK channel identification clock TXSYNC of the transmitter section to the corresponding RXCLK, RXSYNC of the receiver section respectively.  
7)	Set the amplitude of the input sine wave desired.   
8)	Take the observations as mentioned below.

### Kit Diagram
<img width="480" height="324" alt="image" src="https://github.com/user-attachments/assets/b2f8f171-ceff-4481-add2-bc4ef1c42506" />

### Model Graph
<img width="718" height="933" alt="image" src="https://github.com/user-attachments/assets/09c11b43-dc9c-4bf9-a04a-0728d0b0fea3" />

### Program
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt

fs = 100000
t = np.arange(0, 0.01, 1/fs)

# Message & carrier frequencies
fm = [100, 200, 300, 400]
fc = [5000, 10000, 15000, 20000]

# Message signals
m = [0.5*np.sin(2*np.pi*f*t) for f in fm]

# Modulated DSB-SC signals
s = [m[i] * np.cos(2*np.pi*fc[i]*t) for i in range(4)]

# FDM combined
fdm = sum(s)

# Low-pass filter
def lpf(x, cutoff):
    b,a = butter(5, cutoff/(fs/2), 'low')
    return filtfilt(b,a,x)

# Demodulation + LPF recovery
rec = [ lpf(fdm * (2*np.cos(2*np.pi*fc[i]*t)), fm[i]*2) for i in range(4) ]

# ----------------- PLOTTING -----------------
plt.figure(figsize=(13,13))

# FDM
plt.subplot(8,1,1); plt.plot(t, fdm, 'k'); plt.title("FDM - Multiplexed Signal")

# CH0
plt.subplot(8,1,2); plt.plot(t, s[0], 'r'); plt.title("CH0 - Modulated Signal")
plt.subplot(8,1,3); plt.plot(t, rec[0], 'r'); plt.title("CH0 - Recovered Signal")

# CH1
plt.subplot(8,1,4); plt.plot(t, s[1], 'g'); plt.title("CH1 - Modulated Signal")
plt.subplot(8,1,5); plt.plot(t, rec[1], 'g'); plt.title("CH1 - Recovered Signal")

# CH2
plt.subplot(8,1,6); plt.plot(t, s[2], 'b'); plt.title("CH2 - Modulated Signal")
plt.subplot(8,1,7); plt.plot(t, rec[2], 'b'); plt.title("CH2 - Recovered Signal")

# CH3 (Recovered Only)
plt.subplot(8,1,8); plt.plot(t, rec[3], 'm'); plt.title("CH3 - Recovered Signal")

plt.tight_layout()
plt.show()
```

### Tabulation

<img width="1280" height="581" alt="image" src="https://github.com/user-attachments/assets/f1d60ff1-acce-4fca-aafd-ccebcadb51b0" />

### Output
<img width="1289" height="1289" alt="image" src="https://github.com/user-attachments/assets/34226294-95d5-43d6-86b3-4b1abf22af46" />



### Result
Thus the time division multiplexing is done experimentally and output is verified



