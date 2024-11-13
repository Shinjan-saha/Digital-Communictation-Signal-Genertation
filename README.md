# Digital Signal Modultation  Genertation

#### Genertation of ASK,FSK,PSK,DPSK from Binary Signal


## Code

```bash
import numpy as np
import matplotlib.pyplot as plt


fs = 1000  
f_carrier = 10  
bit_duration = 1  


binary_input = input("Enter a binary sequence (e.g., 10110010): ")
binary_data = np.array([int(bit) for bit in binary_input if bit in '01'])  
num_bits = len(binary_data)  
t = np.linspace(0, bit_duration * num_bits, fs * num_bits)


binary_signal = np.repeat(binary_data, fs)


carrier_ask = np.sin(2 * np.pi * f_carrier * t)
carrier_fsk1 = np.sin(2 * np.pi * (f_carrier + 5) * t)  
carrier_fsk2 = np.sin(2 * np.pi * (f_carrier - 5) * t)  
carrier_psk = np.sin(2 * np.pi * f_carrier * t)  
carrier_qpsk = np.sin(2 * np.pi * f_carrier * t)  


ask_signal = binary_signal * carrier_ask


fsk_signal = np.where(binary_signal, carrier_fsk1, carrier_fsk2)


psk_signal = np.sin(2 * np.pi * f_carrier * t + np.pi * binary_signal)


dpsk_signal = np.zeros_like(binary_signal, dtype=float)
current_phase = 0
for i in range(len(binary_signal)):
    current_phase += np.pi * binary_signal[i]
    dpsk_signal[i] = np.sin(2 * np.pi * f_carrier * t[i] + current_phase)


qpsk_signal = np.zeros_like(t)
for i in range(num_bits // 2):
    bit_pair = binary_data[2 * i:2 * i + 2]
    phase_shift = (2 * bit_pair[0] + bit_pair[1]) * (np.pi / 2)
    qpsk_signal[i * fs:(i + 1) * fs] = np.sin(2 * np.pi * f_carrier * t[i * fs:(i + 1) * fs] + phase_shift)


plt.figure(figsize=(15, 10))


plt.subplot(6, 1, 1)
plt.plot(t, binary_signal, color='k')
plt.title("Binary Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")


plt.subplot(6, 1, 2)
plt.plot(t, ask_signal, color='b')
plt.title("Amplitude Shift Keying (ASK)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")


plt.subplot(6, 1, 3)
plt.plot(t, fsk_signal, color='g')
plt.title("Frequency Shift Keying (FSK)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")


plt.subplot(6, 1, 4)
plt.plot(t, psk_signal, color='r')
plt.title("Phase Shift Keying (PSK)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")


plt.subplot(6, 1, 5)
plt.plot(t, dpsk_signal, color='purple')
plt.title("Differential Phase Shift Keying (DPSK)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")


plt.subplot(6, 1, 6)
plt.plot(t, qpsk_signal, color='orange')
plt.title("Quadrature Phase Shift Keying (QPSK)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")

plt.tight_layout()
plt.show()

```

## Terminal Input
```bash

Enter a binary sequence (e.g., 10110010): 101011
```



## Output

<img src="./img/Screenshot 2024-11-13 at 9.17.01 PM.png">

