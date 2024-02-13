---
attachments: [Clipboard_2023-05-10-22-36-44.png, Clipboard_2023-05-10-22-54-01.png, Clipboard_2023-05-11-16-26-11.png, Clipboard_2023-05-11-16-56-11.png, Clipboard_2023-05-11-17-11-16.png, Clipboard_2023-05-11-17-38-30.png, Clipboard_2023-05-11-17-40-33.png, Clipboard_2023-05-11-17-42-18.png]
title: 'Chapter 4: Transceiver Architectures'
created: '2023-05-11T05:28:01.116Z'
modified: '2023-05-12T01:25:16.976Z'
---

# Chapter 4: Transceiver Architectures
## General Considerations
|Issue|Description|
|-----|-----|
|Channel and Band Selection | On transmitter side, filter passband loss can result in power wasted. On receiver side, loss before LNA can increase cascaded NF (loss = NF). Filters that do not have high enough selectivity can inadequately reject other jammers because its rejection could be not enough or the passband is too wide. |
|TX-RX Feedthrough| ![](@attachment/Clipboard_2023-05-10-22-36-44.png) This happens when a duplexer for FDD only provides some rejection but not enough to warrant relaxation of LNA compression point.|
|Image Issue for Heterodyne Receivers| ![](@attachment/Clipboard_2023-05-10-22-54-01.png) This happens when there are two signals at $IF = \omega_l-\omega_i$ and at $IF = Image - \omega_l$. Based on above image, the desired signal and the corresponding image both land on the same IF frequency. As a result, the image can corrupt the desired received signal. This can be mitigated by using an image-rejection filter before the mixer. ![](@attachment/Clipboard_2023-05-11-16-26-11.png) The above diagram shows trade off between high and low IF value selection. A high IF value makes it easier to reject an image as it is farther away from desired frequency but it is harder to design a highly selective or high Q filter at higher frequencies. A low IF value makes it easier to design a selective filter at baseband, but at the cost of less rejection at the image frequency.|
Dual Downconversion | ![](@attachment/Clipboard_2023-05-11-16-56-11.png)In lieu of having only one stage for receivers, another stage is added for downconversion to address the issues that come with higher IF frequency selection. However, each stage must address any respective image related issues. __This is sometimes loosely stated as “every dB of gain requires 1 dB of prefiltering,” or “every dB of prefiltering relaxes the IP3 by 1 dB.”__
| Mixing Spurs | A mixer in reality can introduce undesired spurs. Employing superheterodyne architecture can cause for some of these spurs to get downconverted to desired IF frequency.|
LO Leakage | ![](@attachment/Clipboard_2023-05-11-17-11-16.png) Especially for Zero-IF or Direct Conversion receivers, LO leakage is critical. As seen above, the LO signal can couple via the board parasitics or device parasitics, looping its way back to the input of the receiver. This can then be downconverted to zero IF, causing desensitization.|
IQ Mismatch | ![](@attachment/Clipboard_2023-05-11-17-38-30.png) A difference in delay between IQ waveforms can result in phase mismatch (eg $cos(\omega(t + 10ps))$, then multiply 10ps by $2\pi 5GHz$ can result in as much as 18degrees of mismatch). ![](@attachment/Clipboard_2023-05-11-17-40-33.png) If I and Q are not normalized properly and one of the waveform amplitudes is essentially higher than the other, the constellation can be deformed in this way. ![](@attachment/Clipboard_2023-05-11-17-42-18.png) Phase mismatch can corrupt the constellation in this manner.

