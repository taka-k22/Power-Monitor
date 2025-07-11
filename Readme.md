# Low-Cost, Easy-to-Build General-Purpose Power Outage Alert

## Overview

This device detects power outages in commercial electricity and sends **notifications via LINE**.  
It is ideal for monitoring outages in experimental observation equipment (biology, chemistry, physics, etc.).

> ⚠️ **Caution**: Do not use this device with equipment where malfunction could cause serious damage.

---

## Principle

The system detects power outages using the GPIO input of a Raspberry Pi.  
Since AC 100V cannot be input directly, it is **converted to DC via an AC adapter and USB cable**, then connected to the Pi.  
When the voltage drops below approximately 3V, it is recognized as a "power outage".

---

## Required Items

- Raspberry Pi Model 3 or 4  
- Pass-through mobile battery (capable of charging while supplying power)  
- AC100V → USB adapter + USB cable (100-yen shop is fine)  
- Two wires (to connect USB to Pi)  
- Cutter knife (for cutting and stripping the USB cable)  
- LINE account  

> ※ Raspberry Pi setup (display, keyboard, Wi-Fi, etc.) is assumed to be completed.  
> ※ Two power outlets are required (one for power, one for detection). Use a power strip if necessary.

---

## Steps

1. **Construct the power path for the Pi**  
   Keep the mobile battery connected to an outlet and power the Pi from it. The Pi will remain active during outages.

2. **Modify the USB cable**  
   Cut one end of the cable and extract the red and black wires (or two main lines), strip them, and extend with extra wire. Insulate with tape or heat shrink tubing.

3. **Connect to Raspberry Pi**  
   Plug the USB end into the AC adapter, and connect the wire ends to the Pi GPIO pins:  
   - Red wire → 7th pin from top left (GPIO 4)  
   - Black wire → 8th pin from top left (GND)

4. **Obtain LINE Notify Token**  
   1. Visit [LINE Notify](https://notify-bot.line.me/ja/)  
   2. Log in → My Page → "Generate Token"  
   3. Enter a token name and select the recipient (1-on-1 or group)  
   4. **Copy the displayed access token**

5. **(For group notifications) Invite LINE Notify to the group**

6. **Prepare the script**  
   Download `main.py` from this repository, transfer it to the Pi, and paste your token into line 12: `TOKEN = "..."`.

7. **Run the script**  
   Plug in the AC adapter and run `main.py` using Thonny or similar.  
   - If the console shows "Monitoring active", it's working  
   - If you unplug the adapter, you should receive a LINE message: "Power outage detected..."

---

## Tips

- Many AC adapters contain capacitors that **continue supplying power for a few seconds**, which may delay the alert slightly.
- The **notification interval is set in line 39** of `main.py` (unit: seconds).  
  Default is `600` (10 minutes). During an outage, messages are sent repeatedly at this interval until recovery.

---

## License

MIT License
