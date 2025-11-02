## Topology Selection and Justification

I selected a Sallen-Key low-pass filter due to its simplicity and effectiveness for converting PWM signals into smooth DC voltages. The Howland current pump is chosen for its ability to deliver a load-independent, precise voltage-controlled current source, essential for biomedical stimulation where current stability is critical.

***

## Key Calculations

The cutoff frequency of the Sallen-Key filter is calculated as:

$$
f_c = \frac{1}{2\pi RC} \approx 1.59\,\text{Hz}
$$

with R = $10\,k\Omega$, C = $10\,\mu F$. This ensures effective smoothing for PWM input frequencies between 1 Hz and 200 Hz.

The Howland current pump output current is given by:

$$
I_{\text{out}} = \frac{V_{\text{DC}}}{R}
$$

where $V_{\text{DC}}$ is the average voltage from the filter output and R is the current sensing resistor (600 Ω here).

For example, a 4 V PWM amplitude with a 60% duty cycle at 100Hz PWM frequency yields an average voltage of:

$$
4\,\text{V} \times 0.6 = 2.4\,\text{V}
$$

and an output current of:

$$
I_{\text{out}} = \frac{2.4\,\text{V}}{600\,\Omega} = 4.0\,\text{mA}
$$

I selected ${600\,\Omega}$ to ensure that we get desired output current of 4.0mA for 4 V PWM at 100Hz frequency.
***

## Power Supply Requirements and Choice

The filter op-amp was powered from ±15 V, sufficient for the analog filtering task, while the Howland current pump op-amp was powered from ±35 V to provide the necessary compliance voltage for stable current regulation, even under varying load conditions and voltage swings.

***

## Simulation Method and Approach

I used LTspice transient analysis (.tran) to simulate the whole system. Tests included varying PWM frequency from 1 Hz to 200 Hz, pulse widths between 0.5 ms and 9.5 ms, and sweeping load resistances from 1 kΩ to 10 kΩ. Output currents and voltage waveforms were measured and analyzed for ripple, noise, accuracy, and load independence.

***

## Achieved Results Versus Target Specifications

I successfully demonstrated that the output current remains nearly constant across a wide load resistance range (1 kΩ to 10 kΩ), verifying the Howland current pump's load independence.

Ripple and noise levels in the output current were minimal and within acceptable limits for biomedical applications.

However, the output current exhibited unexpected variation with PWM frequency due to how the PWM signal's pulse width and period were defined in the simulation (fixed “on-time” with variable period), causing the DC average voltage feeding into the current source to change with frequency.

***

## Limitations and Trade-Offs

**Frequency Problem: Effect of Pulse Definition**

This isn't a ripple effect; I fundamentally changed the DC average voltage (Vout from U1) that feeds my current source.

The problem lies in my PULSE definition where “on-time” parameters are fixed, but the total period varies.

With fixed rise (1 ms), on (5 ms), and fall (1 ms) times, the total “up” time is always 7 ms, resulting in a constant voltage-time area.

As I vary the PWM frequency by changing the period from 5 ms to 50 ms, the average voltage—which controls output current—varies inversely.

For example:

- At 100 Hz (T_period = 10 ms):

$$
V_{DC\_avg} = \frac{24\,V\cdot ms}{10\,ms} = 2.4\,V, \quad I_{LOAD} = \frac{2.4\,V}{600\,\Omega} = 4.0\,mA
$$

- At 20 Hz (T_period = 50 ms):

$$
V_{DC\_avg} = \frac{24\,V\cdot ms}{50\,ms} = 0.48\,V, \quad I_{LOAD} = \frac{0.48\,V}{600\,\Omega} = 0.8\,mA
$$

- At 200 Hz (T_period = 5 ms):

Here the pulse cannot complete the defined 7 ms up-time within 5 ms period, truncating the pulse.

$$
V_{DC\_avg} = 3.5\,V, \quad I_{LOAD}  \approx 5.8\,mA
$$

This discrepancy explains why output current changes with frequency.

***

## Future Improvements and Recommendations

To overcome these frequency-dependent effects, I should adjust my PWM signal generation strategy to maintain a **constant duty cycle across frequencies**, rather than a fixed on-time.

Additionally, I may reduce filter cutoff frequency or design higher-order filters to improve ripple attenuation and minimize output variations.

Finally, exploring digital PWM generation or microcontroller DAC techniques could enhance system accuracy and flexibility.

