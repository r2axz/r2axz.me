---
layout: post
title:  "LTspice .NET Statement"
date:   2022-03-07 11:00:00 +0300
categories: ltspice net s-parameters
---
A while ago, I asked a few fellow hams to solve a simple circuit problem shown on the schematics below.

![VNA tool](/assets/2022-03-07-ltspice-net-statement/1.svg){: .schematics}

The question is, what would a calibrated VNA show on the S21 plot if we
connected an ideal 500-ohm resistor between its ports. Surprisingly, only a few
were able to come up with the correct answer. This is especially strange because
nowadays, everyone seems to use [NanoVNA](https://nanovna.com), and you may
expect some degree of understanding of the VNA fundaments. Anyway, let's see
what went wrong.

Since the circuit does not contain any frequency-dependent or distributed
components, a simple DC analysis will do the job. First, let's apply
[KVL](https://en.wikipedia.org/wiki/Kirchhoff%27s_circuit_laws)

{% katex display %}
V - V_{R_S} - V_{R} - V_{R_L} = 0 \tag{1}
{% endkatex %}

{% katexmm %}
where $V$ is the voltage of the voltage source inside Port 1, $V_{R_S}$ is the voltage
drop on the Port 1 source impedance ($R_S$), $V_R$ is the voltage drop on the resistor R,
and $V_{R_L}$ is the voltage drop on the Port 2 load impedance ($R_L$).

Since [$V = IR$](https://en.wikipedia.org/wiki/Ohm%27s_law), equation (1) can be rewritten as
{% endkatexmm %}

{% katex display %}
V - IR_S - IR - IR_L = 0 \Rightarrow\newline
\Rightarrow V = I(R_S + R + R_L) \tag{2}
{% endkatex %}

which gives

{% katex display %}
I = \frac{V}{R_S + R + R_L} \tag{3}
{% endkatex %}

To solve the problem, we must find the voltage on {% katex %}R_L{% endkatex %}

{% katex display %}
V_{R_L} = IR_L = \frac{R_LV}{R_S + R + R_L}\tag{4}
{% endkatex %}

Substituting the values from schematics into equation (4) gives

{% katex display %}
V_{R_L} = \frac{1}{12}V \tag{5}
{% endkatex %}

Those who had succesfully arrived to this point would then most of the times
calculate S21 logmag with the following math

{% katex display %}
S21\_Logmag = 20log_{10}(\frac{\frac{1}{12}V}{V}) \approx\newline\approx -21.58\ dB \tag{6}
{% endkatex %}

**Which unfortunately is completely wrong in the VNA context.**

(to be continued)
