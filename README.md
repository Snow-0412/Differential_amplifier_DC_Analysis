# Differential_amplifier_DC_Analysis
This is a small side project where I built a basic NMOS differential pair in LTspice and ran a `.op` analysis to check that the DC bias point actually matches what you'd calculate by hand. I wanted to make sure I actually understand the bias math for a diff pair before moving on to anything more complicated (current mirrors, cascodes, etc.), so this is basically a sanity check circuit.

# The circuit
<img width="1916" height="878" alt="image" src="https://github.com/user-attachments/assets/1dd93ca7-4692-4b8b-88b0-aacef00b2309" />

Pretty standard textbook diff pair setup:

- Dual supply, VDD = +5V, VSS = -5V
- Two 25k drain resistors (RD1, RD2)
- A 20k resistor for the tail instead of a current source (more on this below)
- Two matched NMOS devices, model params VTO = 1V, KP = 100e-6
- Two sine sources on the gates, 20mV amplitude, 1kHz, 180 degrees apart from each other so they act as a differential input

At t = 0 both inputs are sitting at 0V, so what the `.op` analysis gives you is really just the common-mode bias point of the circuit, not the response to an actual differential signal.

# Checking it by hand

Since both gates are at 0V:

V_GS = V_G - V_S = 0 - (-2.18614) = 2.18614 V
V_OV = V_GS - V_TO = 1.18614 V
I_D = (KP/2) * V_OV^2 = (100e-6 / 2) * (1.18614)^2 ~= 70.35 uA

Then:

V_D1 = VDD - I_D * RD1 = 5 - (70.35uA)(25k) ~= 3.241 V

Which matches what LTspice spat out.

# Results from the simulation
`op_point_results.png` - screenshot of the `.op` output

# Things I know are not ideal about this circuit

- The tail is just a resistor, not a current source. This works fine for checking the DC math, but it's not how you'd actually bias a diff pair if you cared about CMRR or PSRR. A real design would use a current mirror (probably cascoded) for the tail instead.
- This is DC only right now, no AC or transient sweep yet, so I haven't actually looked at the differential gain or common-mode rejection numbers. That's the next step.
  
# Files in this repo

- `Differential_amplifier_DC.asc` - the schematic
- `op_point_results.png` - screenshot of the `.op` output
