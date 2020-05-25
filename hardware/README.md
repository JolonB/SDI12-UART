# Hardware

The purpose of this document is to provide information about the different components used in the systems.
By breaking them down, it means the sections are modularised and can be built up to create the same functionality as required for any microcontroller.

This section may be a little bit theory heavy. Working is shown in case there's a mistake so people can correct it.
However, if you want to skip straight to the implementation, go to the last equation.

## Level shifter 3.3 -> 5

The most energy efficient level shifter design I could come up with is shown below.
It uses two inverting stages to lower the current draw as much as possible.
This level shifter is unidirectional as there is no need for it to be bidirectional.

![Level shifter](img/level_shifter.png)

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

This is the current when <img src="https://render.githubusercontent.com/render/math?math=V_{gs} = 3.3 \text{V}">

We will call the nMOS with the resistor <img src="https://render.githubusercontent.com/render/math?math=n1"> and the nMOS and pMOS <img src="https://render.githubusercontent.com/render/math?math=n2"> and <img src="https://render.githubusercontent.com/render/math?math=p"> respectively from now on.
