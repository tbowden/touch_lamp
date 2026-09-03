# touch_lamp

------------------------------
## 💡 DIY Capacitive Touch Lamp Project Notes## 🛠️ Touch Pad & Wire Assembly (Soda Cans)

* Forget Soldering & Chemicals: Aluminum from soft drink cans is too thin and melts or warps easily under a soldering iron. Skip the potassium hydroxide.
* Surface Prep: Sand down the edges of the aluminum sheets thoroughly. You must scratch off the outer paint and the internal clear plastic liner to expose bare, shiny metal, or electricity won't flow.
* Seamless Joining: Overlap the sanded edges and use the Fold-and-Crimp Method (fold the seam over itself twice and crush flat with pliers) or use Conductive Adhesive Copper Tape over the seam. This keeps the touch pad perfectly flat against the inside of the ceramic stand.
* Wire Connection: Solder your copper wire to a small ring terminal (eyelet). Secure it to the aluminum sheet using a small stainless steel nut and bolt.
* Corrosion Prevention: Coat the final bolted joint in petroleum jelly (Vaseline) or grease. This seals out oxygen and stops galvanic corrosion between the dissimilar metals (copper and aluminum).

------------------------------
## ⚡ Electronics & Sensor Integration## 1. Touch Sensor (TTP223 Module)

* Wire Attachment: Solder the copper wire coming from the aluminum pad directly to the unmasked trace ring or the external antenna pad (labeled "I") on the TTP223 module.
* Sensitivity Tuning: Because the soda-can pad is massive, the sensor might get stuck "ON". If this happens, solder a tiny capacitor (0 to 50 pF) across the empty C1 / Cs pads on the module to lower the sensitivity.
* Jumper Configuration: Leave pads A and B un-soldered (Default/Momentary mode). Let the ESP32 handle the toggle logic in software.

## 2. Microcontroller (ESP32-H2)

* Logic Levels: Power the TTP223 module from the ESP32's 3.3V pin (not 5V). This keeps the sensor's output signal safe for the H2's 3.3V-tolerant GPIO pins.

## 3. Relay Selection (Switching to Solid State)

* The Blue Cube Trap: The popular blue relay modules are electromechanical, not solid-state. The physical spark inside them creates massive electromagnetic interference (EMI) that will cause your giant aluminum antenna pad to false-trigger.
* Recommended SSRs: Swap the blue cube for a true Solid State Relay (SSR) to ensure completely silent, spark-free operation:
* Omron G3MB-202P Module: Cheap, compact, handles up to 2A (480W), and triggers reliably on 3.3V logic.
   * Fotek SSR-25 DA Block: Heavy-duty, highly reliable screw terminals, accepts a 3V–32V DC input.

------------------------------
## 🔒 Crucial Safety & Interference Mitigation

* Physical Isolation: Maintain your planned 5–10 cm gap between the aluminum sheet/TTP223 and the high-voltage mains components at the bottom of the lamp. Keep the ESP32 and relay safely inside your 3D-printed enclosure.
* Mains Power Safety: Avoid using cheap, bare-board AC-DC step-down modules (like generic Hi-Link clones) to power the ESP32. If they fail, they can leak 240V AC onto the low-voltage side. Instead, safely wire a enclosed branded USB phone charger brick inside the base to supply isolated 5V power.
* Software Debounce: In your ESP32 code, implement a strict lockout timer. When a touch is registered and the relay switches state, program the ESP32 to completely ignore the TTP223 input pin for 500ms to 1000ms to let any residual electrical noise settle.

------------------------------

Here is the additional markdown summary for your project notes, specifically tailored to adding the dimming functionality safely with your ESP32-H2. You can append this directly to the bottom of your previous summary.
------------------------------
## 💡 Phase 2 Upgrade: Multi-Level Dimming Functionality## 1. Hardware Shift (Ditch the Standard Relay)

* No Standard SSRs: Standard Solid State Relays (like the Omron G3MB-202P) cannot dim AC loads. Attempting to use them with PWM will cause the LED globe to strobe or buzz violently.
* Use a TRIAC Dimmer Module: Buy an isolated AC Dimmer Module (often branded as RobotDyn or TRIAC Dimmer Module) designed for microcontrollers.
* How it Works: The module reads the 240V AC sine wave and uses a Zero-Cross (ZC) pin to tell the ESP32 exactly when the wave crosses 0 Volts. The ESP32 then pulses the GATE/PWM pin to cleanly "cut" the AC wave, altering the power delivered to the LED globe.

## 2. Dimmer Module Wiring to ESP32-H2

* VCC: Connect to the ESP32 3.3V pin to ensure all logic lines match the H2's voltage tolerances.
* GND: Connect to ESP32 GND.
* ZC (Zero-Cross): Connect to an interrupt-capable digital GPIO pin on the ESP32-H2. Your code will monitor this pin to time the AC dimming cycle.
* GATE / PWM: Connect to a separate digital GPIO pin on the ESP32-H2. The code will toggle this pin to trigger the dimming action.

## 3. USB Cable Management Inside the Base

* Do Not Shorten: USB cables contain fragile shielded wires that are highly problematic to splice cleanly. Leave the cable intact.
* The "Figure-8" Loop Method: Bundle any excess USB cable length in a tight Figure-8 pattern (or zig-zag accordion fold) rather than a uniform circle. Secure the middle with a cable tie. This structural geometry forces the magnetic fields to cancel each other out, preventing the coiled cable from acting as an inductor antenna that causes false touch triggers.

## 4. 240V AC Mains Splicing & Physical Safety

* No Electrical Tape Splices: Under no circumstances should you twist 240V mains wires together and wrap them in tape inside the 3D-printed base.
* Use WAGO 221 Lever Connectors: Use genuine WAGO 221 lever-nuts to splice the incoming active, neutral, and earth lines safely. They provide a secure, enclosed mechanical connection.
* Mechanical Strain Relief: Design or install a physical cable clamp where the mains power cord enters the 3D-printed enclosure. If the cord is yanked externally, the mechanical tension must pull against the plastic housing, never against the internal screw terminals of the dimmer module.

------------------------------
