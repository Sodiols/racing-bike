# Motorcycle Physics and Calibration Specification

**Status:** Conceptual engineering plan only. No equations have been implemented or tested.

## Simulation ambition and limits

We are building a **believable motorcycle simulator**, not claiming measured tire/engine fidelity equivalent to an OEM engineering tool. A motorcycle requires more than rigid body gravity. Balance, countersteering, tire slip, rider input and lean stability need explicit models and possibly documented control assists, especially with keyboard and touch input.

## State variables (proposed)

Engine angular velocity (rad/s), clutch engagement, selected gear, wheel rotational speeds, vehicle position/velocity, yaw/pitch/roll and rates, steering command, rider assist state, brake commands, suspension deflections, road material and surface contact state. Record units in all interfaces.

## Core drivetrain relations

Engine torque `T_e(rpm, throttle)` from a documented torque curve and throttle model.

Gearbox and final ratio `G_total = G_primary * G_gear * G_final`.

Ideal wheel torque `T_wheel = T_e * G_total * eta` with drivetrain efficiency `eta` as an explicit calibration parameter. Clutch slip and inertia require their own model; do not apply this rigid relation while clutch is decoupled.

Drive force limited by traction: `F_drive = min(T_wheel / r_eff, F_traction_available)` for a simplified longitudinal approximation. The traction model must account for normal load, road grip and combined corner/brake slip.

Aerodynamic drag: `F_drag = 0.5 * rho * C_dA * v^2` acting opposite longitudinal velocity. Rolling resistance approximately `F_rr = C_rr * m * g` on suitable surfaces.

Longitudinal acceleration `a_x = (F_drive - F_drag - F_rr - F_brake - grade_resistance) / m_effective`, with sign conventions and rotation inertia documented.

For a steady constant-radius turn, idealized lean condition `tan(lean) = v^2 / (r * g)` is a useful sanity check **but not a full steering model**. Rider position, camber, transient dynamics and tire slip affect actual behavior.

Power `P = T_e * omega`. Use unit conversions carefully; wheel speed, gear ratio and motor RPM must be consistent across gears.

## Model implementation sequence

1. Single-axis powertrain with unit-tested speed/RPM and torque relationships.
2. Engine inertia, throttle lag, idle, limiter, clutch engagement/disengagement and smooth shift windows.
3. Front/rear brake torque and load-sensitive traction.
4. Planar steering/lean with bounded assist and stable low-speed behavior.
5. Contact/suspension and coupling to Havok world.
6. Optional refinements: slip curves, camber, countersteering transients, wheel lift, ABS/traction control, rider mass transfer.

## Gear semantics

Neutral does not transmit positive engine wheel torque. Changing gear has temporal behavior; there must be distinct clutch/shift states. Automatic mode selects appropriate gears by RPM and load while handling protection. Manual mode supports clutch modulation, inappropriate downshift prevention or documented failure behavior, and throttle/engine-brake differences. User settings may simplify clutch behavior but must label riding assists.

## Calibration data hierarchy

1. Manufacturer primary specifications for engine displacement, published power/torque, weight and gearing if disclosed.
2. Reputable measured road tests or instrumented data for acceleration, braking and top speed.
3. Engineering estimates with assumptions for drag coefficient, effective radius and friction.
4. Fictional vehicle specification when branded model rights or sufficient source data are missing.

Separate **manufacturer claims**, **independently measured results**, and **simulation estimates** in the data. Track year/region/variant and URL/date. Verified speed cannot be inferred solely from advertised horsepower.

## Deterministic verification scenarios

- Stationary in neutral: vehicle does not accelerate when throttle is pressed.
- Engaged first gear: tire force and acceleration have correct direction.
- Raising a gear at constant speed lowers engine RPM when coupled.
- Fully disengaged clutch: engine RPM can change independent of wheel speed.
- Engine limiter prevents unbounded RPM; transmission must not produce NaN.
- Aerodynamic drag grows quadratically with speed under fixed air density and area.
- Brakes always dissipate net kinetic energy in ideal straight line conditions, except terrain effects.
- Repeating an input sequence and initial state gives statistically identical outputs within defined floating point tolerances.
- Frame-rate sweeps (30/60/120 FPS render) do not materially change fixed-timestep trajectory or lap validity.
- Lean command approaches a stable angle under a steady turn with bounded forces.

## Validation outputs

Provide trace plots for RPM, gear, speed, throttle, brake, clutch, lean, slip proxy and acceleration over standard test maneuvers. Compare measured accelerations and top speed against documented targets, define tolerances before claiming calibration, and record divergence. Never quietly hard clamp real speed to match a published number.

## Safety of claims

Do not call the result an exact real-world simulator unless vehicle-specific parameterization and measured accuracy justify that label. This is entertainment simulation, not riding instruction or safety guidance.
