# touch_lamp

Here is a clean, scannable markdown summary of the advice for your ESP32-H2 capacitive touch lamp project. You can copy and paste this straight into your project notes or documentation.
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
Before you start wiring the mains voltage side, let me know what type of light bulb you plan to use and how you plan to splice the AC wires, so we can ensure the layout is perfectly safe!

