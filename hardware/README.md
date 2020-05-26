# Hardware

The purpose of this document is to provide information about the different components used in the systems.
By breaking them down, it means the sections are modularised and can be built up to create the same functionality as required for any microcontroller.

Some sections may be a little bit theory heavy.
Working is shown in case there are mistakes so people can correct them and to provide a bit more information for those interested.
The equations provide help for minimising current in power critical situations (such as battery power, which SDI-12 sensors may be connected to).
However, if you want to skip straight to the implementation, just look at the schematic image.

## Level shifter 3.3 -> 5

The most energy efficient level shifter design I could come up with is shown below.
It uses two inverting stages to lower the current draw as much as possible.
This level shifter is unidirectional as there is no need for it to be bidirectional.

![Level shifter](img/level_shifter.png)  
[Link to simulation](https://www.falstad.com/circuit/circuitjs.html?cct=$+1+0.000005+16.817414165184545+84+5+43%0Aw+272+208+272+192+0%0Af+240+240+272+240+32+1.5+0.02%0Aw+272+144+352+144+0%0Aw+352+144+352+160+0%0Ag+208+256+192+256+0%0AR+208+224+192+224+0+0+40+3.3+0+0+0.5%0AS+224+240+208+240+0+0+false+0+2%0Aw+240+240+224+240+0%0Ag+272+256+272+272+0%0Aw+352+208+384+208+0%0Af+320+240+352+240+32+1.5+0.02%0Aw+320+240+304+240+0%0Af+320+176+352+176+33+1.5+0.02%0Ag+352+256+352+272+0%0Aw+352+208+352+224+0%0Aw+320+176+304+176+0%0Aw+304+176+304+208+0%0Aw+352+208+352+192+0%0Aw+304+208+304+240+0%0Aw+272+224+272+208+0%0Aw+272+208+304+208+0%0Aw+272+160+272+144+0%0AR+272+144+272+128+0+0+40+5+0+0+0.5%0Ar+272+192+272+160+0+1000000%0Ao+22+64+3+4099+5+0.00009765625+0+1%0A38+23+0+1000000+10000000+Resistance%0A)

When <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 3.3 \text{V}">, the lower left nMOS will be in the linear/non-saturation/ohmic/triode region, so the current through it is given by the equation:

<img src="https://render.githubusercontent.com/render/math?math=I_{ds} = \beta((V_{gs}-V_{t})V_{ds} - \frac{V_{ds}^2}{2})">

We will look at the left half of the circuit first.
We know that the current through the resistor is <img src="https://render.githubusercontent.com/render/math?math=I_{R} = \frac{V_{dd} - V_{ds}}{R}"> and that <img src="https://render.githubusercontent.com/render/math?math=I_{R} = I_{ds}"> at all times.

If we equate these and rearrange, we get:

<img src="https://render.githubusercontent.com/render/math?math=V_{ds}^2 - 2(\frac{1}{R\beta}%2B 2V_{gs}-V_{t})V_{ds}%2B 2\frac{V_{dd}}{R\beta} = 0">

We now know the input into the CMOS inverter is equal to <img src="https://render.githubusercontent.com/render/math?math=V_{ds}">.
Assuming the resistance is large (>1k), we know that the actual value of <img src="https://render.githubusercontent.com/render/math?math=V_{ds}"> is the lower solution to the quadratic.
Interestingly, the upper solution is <5V, so is technically valid but would not be the correct solution.
Therefore, we have:

<img src="https://render.githubusercontent.com/render/math?math=V_{ds} = \frac{1}{R\beta} %2B V_{gs} - V_{t} - \sqrt{(\frac{1}{R\beta} %2B V_{gs} - V_{t})^2 - 2\frac{V_{dd}}{R\beta}}">

Therefore, the current through the left branch is:

<img src="https://render.githubusercontent.com/render/math?math=I_{ds} = \frac{V_{dd} - V_{ds}}{R} = \frac{V_{dd} - \left(\frac{1}{R\beta} %2B V_{gs} - V_{t} - \sqrt{(\frac{1}{R\beta} %2B V_{gs} - V_{t})^2 - 2\frac{V_{dd}}{R\beta}}\right)}{R}">

when <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 3.3 \text{V}">

The calculation for when <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 0 \text{V}"> is a bit different in that it sets an upper bound for the resistance provided by the resistor. 
You need to know the value of <img src="https://render.githubusercontent.com/render/math?math=I_{dss}"> for the transistor.
To simplify the explanation, this is the maximum current while the transistor is off and the value can be found on the datasheet.
Knowing this, we need to set the resistance such that <img src="https://render.githubusercontent.com/render/math?math=I_{R} > I_{dss}">.
Remember, <img src="https://render.githubusercontent.com/render/math?math=I_{dss}"> is the max current, so the current through the resistor will be limited to this value if a low enough resistance is selected.
We can write this as <img src="https://render.githubusercontent.com/render/math?math=\frac{V_{dd}-V_{ds}}{R} > I_{dss} \Rightarrow R < \frac{V_{dd}-V_{ds}}{I_{dss}}">.
Assuming this condition is satisfied, then <img src="https://render.githubusercontent.com/render/math?math=V_{ds} = V_{dd} - I_{dss}R">

In short, the current through the left branch is <img src="https://render.githubusercontent.com/render/math?math=I_{ds} = \frac{V_{dd} - V_{ds}}{R} = \frac{V_{dd} - \left(\frac{1}{R\beta} %2B V_{gs} - V_{t} - \sqrt{(\frac{1}{R\beta} %2B V_{gs} - V_{t})^2 - 2\frac{V_{dd}}{R\beta}}\right)}{R}"> when <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 3.3 \text{V}"> and <img src="https://render.githubusercontent.com/render/math?math=I_{ds} = I_{dss}"> when <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 0 \text{V}">.
<img src="https://render.githubusercontent.com/render/math?math=V_{ds}"> can be written as <img src="https://render.githubusercontent.com/render/math?math=V_{ds} = \frac{1}{R\beta} %2B V_{gs} - V_{t} - \sqrt{(\frac{1}{R\beta} %2B V_{gs} - V_{t})^2 - 2\frac{V_{dd}}{R\beta}}"> and <img src="https://render.githubusercontent.com/render/math?math=V_{ds} = V_{dd} - I_{dss}R">, respectively.

The next step is to feed the output of the circuit into the right branch.
This will allow you to find the optimal values to minimise the current from the source because if the output voltage is too low, the pMOS transistor will switch to the linear/saturation region. A pMOS transistor is in the off state when <img src="https://render.githubusercontent.com/render/math?math=V_{gs_p} > V_{t_p}">.
We know that <img src="https://render.githubusercontent.com/render/math?math=V_{gs_p} = V_{ds_{n1}} - V_{dd}"> and <img src="https://render.githubusercontent.com/render/math?math=V_{t_p}"> can be found on the datasheet.
Therefore, we have a requirement that <img src="https://render.githubusercontent.com/render/math?math=V_{ds_{n1}} - V_{dd} > V_{t_p}">.
The overall current of the circuit appears to be the lowest when the pMOS is on the edge of the cutoff region (it's probably best to give it a bit of breathing room though).

There's still a bit more work to do.
To get a rough idea of a resistance to use, look at [this graph](https://www.desmos.com/calculator/3hmn6u5ff4). 
The resistance is on the x-axis and should not be greater than the point at which the red and purple lines intersect.
Past the intersection point, the pMOS is in linear/saturation.
The blue line represents the current (though multiplied by 100,000) through the left nMOS.
We can assume that if the pMOS is in the cutoff region that the right branch has minimal current (<img src="https://render.githubusercontent.com/render/math?math=I = I_{dss_p}">) therefore you only need to minimise the blue current curve.
As mentioned before, give a bit of breathing room. The default configuration in the graph would be best at 6 M&Omega;.
Make sure you play around with the values in Desmos to get the optimal value for your MOSFETs.

## Inverter

![Inverter](img/inverter.png)  
[Link to simulation](https://www.falstad.com/circuit/circuitjs.html?cct=$+1+0.000005+16.817414165184545+84+5+43%0Aw+352+144+352+160+0%0Ag+272+224+256+224+0%0AR+272+192+256+192+0+0+40+5+0+0+0.5%0AS+288+208+272+208+0+1+false+0+2%0Aw+352+208+384+208+0%0Af+320+240+352+240+32+1.5+0.02%0Aw+320+240+304+240+0%0Af+320+176+352+176+33+1.5+0.02%0Ag+352+256+352+272+0%0Aw+352+208+352+224+0%0Aw+320+176+304+176+0%0Aw+304+176+304+208+0%0Aw+352+208+352+192+0%0Aw+304+208+304+240+0%0AR+352+144+352+128+0+0+40+5+0+0+0.5%0Aw+288+208+304+208+0%0Ao+14+64+3+4099+5+0.00009765625+0+1%0A)