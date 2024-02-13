---
attachments: [Clipboard_2023-05-08-22-39-10.png, Clipboard_2023-05-08-22-40-52.png, Clipboard_2023-05-08-23-00-22.png, Clipboard_2023-05-10-20-22-05.png, Clipboard_2023-05-10-20-35-52.png, Clipboard_2023-05-10-20-41-34.png, Clipboard_2023-05-10-20-58-06.png, Clipboard_2023-05-10-21-39-43.png, Clipboard_2023-05-10-22-00-54.png]
title: 'Chapter 3: Communication Concepts'
created: '2023-05-09T05:25:09.251Z'
modified: '2023-05-12T01:19:21.744Z'
---

# Chapter 3: Communication Concepts

## Important Aspects of Modulation
1. Say for BPSK, you have two states - 1 and 0. There is a lot of margin to distinguish one from the other as compared to having 4 different states, that are delineated by amplitude.
2. Bandwidth efficiency - for a given amount of bandwidth transmitted, how many users can use it simultaneously
3. Power efficiency - how much power is useful during the communication process

## Analog Modulation
### Amplitude Modulation
Say you have a sinusoid given at $w_c$ carrier frequency. In addition to this, it is further modulated such as the envelope of this signal corresponds to the message sent. The envelope plays at $w_m$ frequency.
![](@attachment/Clipboard_2023-05-08-22-39-10.png)
where m = modulation index.
![](@attachment/Clipboard_2023-05-08-22-40-52.png)
Note that it is not widely used because it is more susceptible to noise and requires more linear amplifiers, so that the message signal can be retrieved properly by the receiver.

### Phase and Frequency Modulation
If the excess phase of a signal is linearly proportional to the baseband signal, it refers to __phase modulation__. Otherwise, if the excess frequency of a signal is proportional to its baseband signal, then it refers to __frequency modulation__.

![](@attachment/Clipboard_2023-05-08-23-00-22.png)
Source: [here](https://www.quora.com/What-is-the-difference-between-phase-modulation-and-frequency-modulation)

## Digital Modulation

This is all about how to pack more information within the same allotted bandwidth

### Intersymbol Interference
If a sinc function is used on the frequency domain, this will result in a brick wall response per bit in time domain. The issue with this is that leakage from one bit can occur and skew the symbol of the adjacent bits. This effect can be additive, and can cause a 0 to look like a 1.

To combat this, if a sinc shape is used per bit, such its nulls coincide to the peak of subsequent bits, ISI can be mitigated. This will also require a brick wall response on the frequency domain.

![](@attachment/Clipboard_2023-05-10-20-22-05.png)

### Quadrature Phase Shift Keying
![](@attachment/Clipboard_2023-05-10-20-35-52.png)
This is performed by parallelizing the serial information, such that each even bit goes to channel A in above diagram, and the odd bits go to B. Channel A is then upconverted with $cos(\omega_ct)$ and $sin(\omega_ct)$. The bits on channel A are __in phase__ and __quadrature__ for channel B.

When a slight phase mismatch is encountered, after doing some math, the below diagram shows how the constellation can be skewed.
![](@attachment/Clipboard_2023-05-10-20-41-34.png)

### 16-QAM
As compared to QPSK, where a symbol can be either +-1, for 16-QAM, it values can range at +-1 and +-2. As a result, the constellation is shown below:
![](@attachment/Clipboard_2023-05-10-20-58-06.png)
While more information is packed within the same bandwidth, this is more susceptible to noise because of its dependence to amplitude as well. It is preferred to use a more linear amplifier when dealing with this to minimize errors in demodulation.

### Orthogonal Frequency Division Multiplexing
Each narrow subcarrier $f_c$ packs QPSK or 16QAM or whatever digital modulation is used. A peak of a subcarrier can land on the null of an adjacent subcarrier for orthogonality, so that information is not leaked between carriers. 

![](@attachment/Clipboard_2023-05-10-21-39-43.png)

The advantage to using OFDM is that it more immune to multipath fading, as each subcarrier has low data rate over a narrow bandwidth. 

However an issue with OFDM is that the _peak to average ratio_ of a waveform can get higher since we are transmitting so many carriers at the same time. This would cause an amplifier to get compressed earlier. 

In general, there are three causes to a higher PAR:
1. Pulse shaping in the baseband
2. Reliance on amplitude for modulation
3. OFDM

### Spectral Regrowth
Considering that the input of a signal has in-phase and quadrature waveforms, as shown below, spectral regrowth can happen if this signal is input into a non-linear system. Consider the arguments inside the cosine function below, where $3\omega_c$ terms are seen (regrowth part).
![](@attachment/Clipboard_2023-05-10-22-00-54.png)


