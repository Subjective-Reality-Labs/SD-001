# PCB design notes

## Inductor choice

3 of 4 charge ICs on this board use internal buck converter to generate output voltage. This requires a suitable inductor. $I_{sat}$ of selected indutor has to be bigger than peak current of the buck converter that can be calculated using this formula:

$I_{peak} = I_{out} + \frac{((V_{in}-v_{out})*D)}{(2*L*freq))}$ where $D$ is duty cycle: $D = \frac{V_{out}}{V_{in}}$

AW32257 maximum charging current is 2.5A @1.5MHz,

APW7261 is 1.5A @3MHz,

TP5100 is 2A @800kHz.

## Shunt resistors

There is 4 places on this board that need a shunt resistors. 3 of the charging ICs use them to control charging current and then there is one on the MCU side that lets it measure the current going in/out of the battery if it is connected to the corresponding battery terminal.

You have to chose the proper footprint for these kind of resistors so the are rated according to anticipated power dissipation that can be calculated with $P = I^2*R$. For example 33mOhm sense resistor with maximum current flowing though it at 2.5A will have to dissipate 0.21W. 0603 resistors have power rating of 1/8W and 0805 have 1/4W rating. So we need to use 0805 as it's the smallest one that fits the calculated rating.

In the case of TP5100 there I placed 3 resistors in parallel. Firstly since it doesn't have I2C interface for customizing its settings, you can only set charging current via these resistors and using 3 of them in parallel allows to easily achieve needed current target even with limited set of resistor nominals. Secondly this also spreads power dissipation among all of them and you can use multiple of lower rated resistors instead of one high-rated.

