# Flashing and Recovery, Solving Issues on a T-Deck 

As an embedded systems enthusiast, I recently encountered a challenging situation with my ESP32-S3 board, specifically the T-Deck. The device was stuck in a state that prevented me from deploying new code, likely due to a corrupted firmware or a problematic sketch. In this blog post, I'll walk you through the steps I took to recover the board and get it back to a functional state.

## The Problem: Unresponsive ESP32-S3

Imagine you've been working on a project with your ESP32-S3, and suddenly, it stops responding to new code deployments. The board might be stuck in a boot loop, or it might not be recognized by your development environment at all. This can happen due to various reasons, such as:

1. A sketch that interferes with the boot process
2. Corrupted firmware
3. Incorrect partition table
4. Hardware issues

In my case, I needed to perform a complete reflash of the board to restore its functionality.

## The T-Deck: A Unique ESP32-S3 Platform

Before we dive into the flashing process, let's take a moment to appreciate the unique device we're working with: the T-Deck.

![LILYGO-T-Deck-ESP32-S3-LoRa-Module-2-8-inch-LCD-Development-Board-LoRaWAN-Long-Range](https://github.com/contractorwolf/flash_id_project/assets/4690960/237b51ab-4e3f-452a-bfb0-070ca1d9dcaf)



The T-Deck in all its glory - a compact, feature-packed device based on the ESP32-S3.

The T-Deck is an innovative handheld device that combines the power of the ESP32-S3 with a range of integrated peripherals, making it a versatile platform for various projects. Some key features include:

1. A compact QWERTY keyboard
2. A 2.8-inch IPS LCD screen
3. A small joystick for navigation
4. Built-in Wi-Fi and Bluetooth capabilities
5. A microSD card slot for expandable storage
6. 
![f10211753ea94f702918c1ef9fa1329b5da25d8a](https://github.com/contractorwolf/flash_id_project/assets/4690960/d426e7f5-e943-4e14-a52b-b179f2904367)

The T-Deck's compact keyboard and vibrant display make it ideal for portable projects.

When working with the T-Deck, it's important to remember that while it's based on the ESP32-S3, its unique form factor and integrated components may require special considerations during the flashing and development process.

## The Solution: Full Firmware Reflash

After trying various troubleshooting steps, I found that a full reflash of the firmware was necessary. Here's a detailed breakdown of the process I used:

1. **Prepare Your Environment**: 
   I used the Espressif IDF (IoT Development Framework) for this process. Make sure you have the latest version installed on your system.

2. **Connect Your Board**:
   I connected my ESP32-S3 to my computer via USB and identified the correct COM port (in my case, COM7).

3. **Navigate to Your Project Directory**:
   Open a command prompt and navigate to your project directory. In my case:
   ```
   C:\esp\flash_id_project>
   ```

4. **Run the Flash Command**:
   I used the following command to flash the board:
   ```
   idf.py -p COM7 -D IDF_TARGET=esp32s3 flash
   ```
   This command does several things:
   - `-p COM7` specifies the port
   - `-D IDF_TARGET=esp32s3` explicitly sets the target device
   - `flash` tells the tool to perform a full flash operation

5. **Monitor the Flashing Process**:
   The output will show you the progress of the flashing operation. Here's a breakdown of what happened:

   a. The tool compiled the project and generated the necessary binary files.
   b. It connected to the ESP32-S3 chip, identifying its features.
   c. The flash was erased in specific regions:
      - 0x00000000 to 0x00005fff (bootloader)
      - 0x00010000 to 0x00048fff (main application)
      - 0x00008000 to 0x00008fff (partition table)
   d. It then wrote the bootloader, application, and partition table to their respective addresses.
   e. After each write, it verified the hash of the data.

6. **Reset and Verify**:
   After the flashing process, the tool performed a hard reset of the board via the RTS pin.

## Understanding the Output

Let's break down some key information from the output:

1. **Binary Sizes**:
   - The main application binary size was 0x38410 bytes (about 230KB).
   - The bootloader binary size was 0x5250 bytes (about 21KB).

2. **Flash Utilization**:
   - The smallest app partition is 0x100000 bytes (1MB).
   - The application used about 22% of this space, leaving 78% free.

3. **Flashing Speed**:
   - The bootloader was written at about 490.7 kbit/s.
   - The main application was written at about 1018.9 kbit/s.
   - The partition table was written at about 556.4 kbit/s.

## Additional Considerations for T-Deck

When flashing or developing for the T-Deck, keep these points in mind:

1. **USB Connection**: The T-Deck typically uses a USB-C port for connection to your computer. Ensure you have a compatible cable.

2. **Power Management**: The T-Deck has a built-in battery. Make sure it's sufficiently charged before beginning the flashing process.

3. **Bootloader Mode**: To enter bootloader mode on the T-Deck, you may need to hold a specific button combination while powering on the device. Consult the T-Deck documentation for the exact method.

![PXL_20240618_145909483](https://github.com/contractorwolf/flash_id_project/assets/4690960/b44bc0ba-aa2e-4f3f-8ea8-b0ea3d76793e)


The T-Deck's side profile, highlighting the USB-C port and programmable buttons.

![PXL_20240619_204657879](https://github.com/contractorwolf/flash_id_project/assets/4690960/27d73ff6-55e8-48ce-afff-0a164431f353)


4. **Display and Keyboard Testing**: After flashing, it's a good idea to test the integrated display and keyboard to ensure all components are functioning correctly.

By keeping these T-Deck-specific considerations in mind, you can ensure a smooth flashing and development process for this unique ESP32-S3 based device.

## Lessons Learned and Best Practices

This experience reinforced several important practices for working with ESP32 and similar microcontrollers:

1. **Always Have a Backup**: Keep copies of your working firmware and important sketches.

2. **Use Version Control**: Tools like Git can help you track changes and revert to working versions if needed.

3. **Understand Your Toolchain**: Familiarity with tools like `esptool.py` and the IDF can be invaluable in troubleshooting.

4. **Keep Your Development Environment Updated**: Using the latest stable versions of tools can help avoid compatibility issues.

5. **Document Your Process**: As I'm doing in this blog post, documenting your troubleshooting steps can help you and others in the future.

## Conclusion

While encountering issues with your ESP32-S3 or similar boards can be frustrating, having the knowledge to perform a full reflash can be a powerful tool in your troubleshooting arsenal. This process essentially gives your board a clean slate, allowing you to start fresh if you encounter seemingly unresolvable issues.

Remember, every challenge you overcome makes you a more skilled developer and embedded systems enthusiast. Don't be afraid to dig deep into the documentation and try advanced recovery methods when simpler solutions fail.

Happy coding, and may your ESP32-S3 projects flourish!



# PocketGPT using the Lilygo T-Deck
## Arduino Tools Settings for the Lilygo T-deck
<a href="images/device-blue.png" target="_blank">
    <img src="images/device-blue.png" width="300">
</a>
<a href="images/devices-green-black.png" target="_blank">
    <img src="images/devices-green-black.png" width="300">
</a>
<a href="images/devices-green-black-r-side.png" target="_blank">
    <img src="images/devices-green-black-r-side.png" width="300">
</a>



PocketGPT is initially built on the ESP32Berry project from 0015:
https://github.com/0015/ESP32Berry


I made significant modifications to fit my use-case which is a custom GPT device that can maintain a converation thread through multiple requests (not available in the ESP32Berry project).
I also needed to use OpenAI Assistants so that I could customize the style and reading levels of my two sons.  I created an OpenAI Assitant for each that would maintain a disposition
and role that was appropriate to their age and would encourage curiosity, learning and reading as well as suggesting follow-up questions to keep the conversation going.  One Assitant speaks properly to a middle school age child, and one to a elementry school child (acts as a one-on-one teacher assitant almost) 





## Arduino Tools Settings for the Lilygo T-deck
<a href="images/arduino-t-deck-settings.png" target="_blank">
    <img src="images/arduino-t-deck-settings.png" width="300">
</a>


reset esp32 when they get stuck:

`C:\esp\flash_id_project>idf.py -p COM7 -D IDF_TARGET=esp32s3 flash`
