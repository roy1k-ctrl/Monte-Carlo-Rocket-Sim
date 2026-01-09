# Monte Carlo Rocket Landing Simulation

A Python-based 2D physics simulation that verifies a vertical landing GNC (Guidance, Navigation, and Control) algorithm using Monte Carlo methods.

This tool simulates 100 flights with randomized wind gusts, sensor noise, and mass variations to test the robustness of a PID controller for a reusable rocket booster.

## Project Overview

The goal of this project was to design a control system capable of landing a reusable rocket (similar to SpaceX's Falcon 9) and strictly verify its reliability under realistic uncertainty.

The simulation includes:
* **Physics Engine:** Custom 2D Equations of Motion (EOM) including variable gravity, atmospheric drag, and thrust vectoring.
* **GNC Logic:**
    * **Guidance:** Keplerian orbit prediction to trigger deorbit burns.
    * **Control:** A Proportional-Derivative (PD) controller regulating throttle and thrust angle.
* **Monte Carlo Analysis:** Statistical verification of the safety margin by running hundreds of scenarios with random error injection.

## Simulation Results

The simulation runs 100 flights to generate a success rate and fuel efficiency distribution.

### 1. Trajectory Analysis (Best vs. Worst Case)
The plot below compares the most efficient landing (Blue) against the least efficient (Red) that experienced heavy wind and sensor noise.

Parameters: Initial Mass: 3500 kg | Empty Mass: 100 kg | Max Thrust: 60000 N

![Trajectory Plot](/img/Trajectory_Comparison_and_Engine_Activity_Comparison.png)

* **Trajectory:** Blue tracks an optimal descent; Red shows the rocket fighting crosswinds to prevent drift.
* **Throttle:** Both profiles show identical saturation at 100% thrust. This confirms the guidance logic relies on Thrust Vectoring (Pitch) rather than throttle modulation to compensate for wind disturbances.
* **Insight:** The system prioritizes maximum deceleration for fuel efficiency, using the engine's gimbal angle, not power, to stabilize the landing.

### 2. Fuel Efficiency Distribution
A histogram showing the remaining fuel across all successful landings. This helps determine the optimal "Safety Margin" for fuel loading.

![Histogram](/img/Fuel_Efficiency.png)

* **Average Fuel Remaining:** Indicates the nominal performance of the system.
* **The Shape of Distribution:** The distribution skews left, showing that a subset of high-stress flights requires significantly more propellant.
* **Design Conclusion:** The long-tail distribution highlights the extra fuel required to compensate for high-wind scenarios. This data can be used to precisely calculate the required fuel safety margin to ensure a successful landing without carrying unnecessary dead weight.

## Technical Details

* **Language:** Python 3
* **Libraries:** `numpy` (Physics/Math), `matplotlib` (Visualization)
* **Integration:** Euler Method (Time step: 0.1s)
* **Controller:** PID (Proportional-Integral-Derivative) Logic
    * Throttle Control: Maintains constant descent velocity near the surface.
    * Vector Control: Tilts the rocket to cancel horizontal drift caused by wind.
