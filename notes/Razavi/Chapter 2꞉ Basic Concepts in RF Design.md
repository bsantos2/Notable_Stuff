---
attachments: [Clipboard_2023-05-06-14-22-41.png, Clipboard_2023-05-06-14-25-56.png, Clipboard_2023-05-06-14-58-31.png, Clipboard_2023-05-06-15-05-52.png, Clipboard_2023-05-07-09-25-14.png, Clipboard_2023-05-07-09-48-00.png, Clipboard_2023-05-07-09-48-22.png, Clipboard_2023-05-07-09-50-41.png, Clipboard_2023-05-07-17-55-30.png, Clipboard_2023-05-07-18-06-36.png, Clipboard_2023-05-07-19-19-30.png, Clipboard_2023-05-07-19-22-45.png, Clipboard_2023-05-07-19-26-30.png, Clipboard_2023-05-07-19-29-59.png, Clipboard_2023-05-07-20-03-39.png]
title: 'Chapter 2: Basic Concepts in RF Design'
created: '2023-05-06T21:20:57.450Z'
modified: '2023-09-28T03:48:42.083Z'
---

# Chapter 2: Basic Concepts in RF Design

## Effects of Nonlinearity
This an approximation for the input and output of memoryless systems 
![](@attachment/Clipboard_2023-05-06-14-22-41.png)

### Harmonic Distortion
If a sine input is applied to a nonlinear systems, the output can generate harmonics.
![](@attachment/Clipboard_2023-05-06-14-25-56.png)
On the final form of the equation, there is a DC component, the desired output, the second harmonic or the $cos(2 \omega t)$ term and the third harmonic or the $cos(3 \omega t)$ term, with A as the amplitude of the input signal.

Observe that increasing the input by 1dB results in increasing second harmonic levels by 2dB. For third harmonic, it results in an increase by 3dB.
~~~
#Select input filter
maintone        = 900e6
power           = 0
gain            = 0
harmonic        = list()
maxLvlForPreamp = -35
inputFilter.setfrequency(maintone)

#Setup sig gen at the desired power and frequency
signalGenerator.output(false)
signalGenerator.setpower(power)
signalGeneartor.setfrequency(maintone)

#Setup spectrum analyzer and filtering
for x in range(2, 10):
  outputFilter.setfrequency(x * maintone)
  approximateRefLevel = power + gain - outputFilter.getLoss(maintone)
  specan.preamp(false)
  specan.reflevel(approximateRefLevel)
  
  if approximateRefLevel < maxLvlForPreamp:
    specan.preamp(true)
  
  signalGenerator.output(true)
  specan.setmarker(x * maintone)
  harmonic.append(specan.getMarkerYvalue + outputFilter.getLoss(x * maintone))
~~~

### Gain Compression
![](@attachment/Clipboard_2023-05-06-14-58-31.png)
It is described as the input power that results in 1dB degradation in gain.

__Why it matters:__ 
- If the modulation is reliant on the amplitude, then clipping could occur when compressed, which results in incorrect information sent or received.
- In a situation where you have a high power jammer close to intended received signal with low power, the jammer could compress receiver gain and output nonlinear signal, reducing the chance for the desired signal to be properly demodulated. This is described as _desensitization_. 
![](@attachment/Clipboard_2023-05-06-15-05-52.png)

#### Brute force code for P1dB
~~~
def p1dBFinder():
  freq       = 500e6
  maxpin     = 10dBm #before device gets destroyed for example
  startpin   = -23dBm
  deltapin   = 0.1dB

  sg.power(startpin)
  sg.freq(freq)
  sg.turnon(true)
  pout = pmout.measure(freq)
  linearGain  = pout - startpin
  currentGain = pout - startpin
  while (linearGain - currentGain) < 1 or pin <= maxpin:
    pin += deltapin
    sg.power(pin)
    pout = pmout.measure(freq)
    currentGain = pout - pin

  if pin == maxpin and linearGai - currentGain < 1:
    print("Error: max pin does not compress LNA enough")
  else
    return pin
~~~

#### If time is a concern, binary search can be used

~~~
def p1dBFinder():
  freq       = 500e6
  maxpin     = 10dBm #before device gets destroyed for example
  startpin   = -23dBm
  deltapin   = 0.1dB

  sg.power(startpin)
  sg.freq(freq)
  sg.turnon(true)
  pout = pmout.measure(freq)
  linearGain   = pout - startpin
  currentGain  = pout - startpin
  midpower     = (startpin + maxpin) / 2
  maxiteration = 100
  while maxiteration > 0:
    pin = midpower
    sg.power(pin)
    pout = pmout.measure(freq)
    currentGain = pout - pin 
    if abs(linearGain - currentGain) <= 0.1:
      return pin
    if (linearGain - currentGain) < 1: #err to upperside
      midpower = (midpower + endpower) / 2
    elif (linearGain - currentGain ) > 1: #err to lowerside
      midpower = (startpin + midpower) / 2
    maxiteration = maxiteration - 1

  if pin == maxpin and linearGai - currentGain < 1:
    print("Error: max pin does not compress LNA enough")
  else
    return pin
~~~
### Cross Modulation
This is defined as when a strong interferer's amplitude information leaks unto desired signal. Phase modulation on the interferer however will not cause cross modulation unto desired signal.
![](@attachment/Clipboard_2023-05-07-09-25-14.png)

### Intermodulation
It is a two-tone test, where the DUT generates unwanted signals at the following frequencies.

__For 2nd order:__
- $f1 + f2$
- $f1 - f2$

__For 3rd order:__
Near $f1$ and $f2$ are:
- $2 f1 - f2$
- $2 f2 - f1$

Others are:
- $2 f1 + f2$
- $2 f2 + f1$

> A common question would be, if you had a series of components that have different levels of linearity in a receiver, how would you arrange them for maximum linear performance?
> Place the most linear component last. In the receiver chain, you may start with switches, filters, low noise amplifiers, mixers. As you go from the antenna to baseband, you increase the power level of the signal, thereby, a more linear device in the later part of the chain is desired to handle this change in amplitude.

On this diagram, it indicates that IIP3 is the input level that causes the intermodulation product to be equal to the output of the device.
![](@attachment/Clipboard_2023-05-07-09-48-00.png)

The issue with the previous diagram is that the gain is linear the whole time. In reality, it resembles below.
![](@attachment/Clipboard_2023-05-07-09-48-22.png)

> A common estimate between $IIP3 = P1dB + 10dB$

![](@attachment/Clipboard_2023-05-07-09-50-41.png)

#### IIP3 Equation
$IIP3 = \Delta Pout/ 2 + Pin$

To measure IIP3, the output power of f1 and f2 are measured, and at $2f1-f2$ and $2f2 - f1$. $\Delta Pout$ is the power difference between intermodulation tone and its nearby main tone. 

>Tips for measurement: 
> 1. Intermod tone must be 10dB above noise floor, otherwise, anything below that is easily too affected by the noise floor.
> 2. Increase main tone by 1dB and its nearby intermod tone should increase by 3dB, unless your tone has been buried in the noise floor.

## Noise Figure
![](@attachment/Clipboard_2023-05-07-17-55-30.png)
Noise figure is a metric that describes the degradation of the signal to noise ratio from a device. If there is no degradation, the NF = 1 (linear) and NF = 0dB.

### Cascaded Noise Figure
![](@attachment/Clipboard_2023-05-07-18-06-36.png)
The above equation shows what the total NF is (linear) after cascading different components in the receiver. It is also called the Friis equation for noise. 

![](@attachment/Clipboard_2023-05-07-20-03-39.png)

> When designing the receiver chain, the initial component must have the lowest NF with a significantly high gain, when referring to above equation. Note that subsequent stages in NF are not as critical because these terms get divided by the previous gain values.

The NF of a lossy component is simply its loss. For example, if a lossy switch exists before the LNA, the resulting cascaded NF becomes in dB
$$NF_t = NF + |Loss|$$

### Measuring NF
A good application note for this can be located [here](https://www.keysight.com/us/en/assets/7018-06829/application-notes/5952-3706.pdf) 
There are multiple ways to measure noise figure:
1. Y-Factor Method
2. Cold Source or Gain Method

#### Y Factor Method
A spectrum analyzer with noise figure application uses this method to measure NF. It entails using a noise source, that has a specified _excess noise ratio (ENR) per frequency_ table. 

The excess noise ratio is determined by toggling the noise source on or off. As explained in the application note, Toff is at 290K. A lower ENR noise source is more suitable for DUTs with known low noise figure for a more accurate measurement, as a larger ENR noise source would skew the results too much, relative to the low noise inherent in the DUT already. 
$$ENR = (Ton - Toff) / Toff$$

The noise source is connected to a 28V square pulse source located at the back of a noise figure meter or a PXA with NF application. The Y factor itself is defined as
$$Y = Ton / Toff$$

![](@attachment/Clipboard_2023-05-07-19-19-30.png)
Above diagram describes the setup for this method. Replacing the DUT or Stage 1 device with a thru connection to connect the noise source directly to the NF meter is the setup for calibration.

![](@attachment/Clipboard_2023-05-07-19-22-45.png)
The above describes the Y factor of the instrument itself, with temperature, T2. Following calibration, the instrument then normalizes to this, such that the displayed effective NF will just be of the DUT.

![](@attachment/Clipboard_2023-05-07-19-26-30.png)
![](@attachment/Clipboard_2023-05-07-19-29-59.png)
With the DUT now inserted back into the setup, the effective Y factor and temperature of the device are described above. Y12 is the output noise level difference (dB) when the noise source is toggled on, then off.

The NF is extracted by using below equation where all values are linear. ENR is looked up from the noise source ENR table.
$$Y = ENR/F + 1$$

#### Gain Method
Note that the definition of NF can further be simplified as 
$$F = SNR_i / SNR_o$$
$$F = N_o / (N_i G)$$

Converting this to dB
$$NF = Power_o - Gain + 174dBm/Hz$$
where gain and power output are both measured before hand before getting the device noise figure. -174dBm/Hz is the noise density at room temperature.

## Sensitivity
This is defined as the minimum amount of power input to a receiver such that it can still demodulate and get a meaningful message.

$$Sensitivity = -174dBm/Hz + SNR_m + 10log(BW) + NF$$



