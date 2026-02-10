# How to Recover Bricked Simos12.1 ECU

**Problem:** Firmware experiments led to a crash of CBOOT. The ECU no longer responds via GPT.
* For each Simos12.1 ECU, the Irom and Erom firmware is unique and the firmware from someone else's block will not work!
* Use only a backup from your ECU! 

---

## 📋 Prerequisites & Tools

**We will need:**

### Hardware
*   **Scanmatik 2**
*   **3-4 resistors of 1000 ohm**
![20260205_200438](https://github.com/user-attachments/assets/87142523-017f-4622-9290-9989dc96d38e)

![20260205_091455](https://github.com/user-attachments/assets/bd10c3a7-512c-4f3f-b6d8-9bff417ffeb7)

![20260205_120836](https://github.com/user-attachments/assets/1ba48fa8-d2b3-4fcf-8255-6b5774945b62)

### Software & Data
*   **PCMflash 53 module Infineon: TC1797 MICRO (4096KB)**
*   **Full backup `amt-bst`** (you will need a password to access the flash)

---

## 🔑 Step 1: Reading the Password

There are 2 ways to read the password:

1.  **Create a `.pwd` file** 
   
      The password for PCMFlash is located in OTP Full Backup at **0x1420c - 0x1421b** example   **10 7F F4 0F B0 A7 BE 72 06 DA 06 01 6C 28 35 BB 14**
      
      Chip id **0x14200-0x1420B**    example **44 80 06 0A 06 40 97 71 74 10 00 10**
       ![20260203_225153](https://github.com/user-attachments/assets/152bbb33-2fa5-487f-89d9-d0b0c552dedb)

     Create a file with the extension **.pwd** and put it there via hexeditor (WinHex,HxD )  **10 7F F4 0F B0 A7 BE 72 06 DA 06 01 6C 28 35 BB 14**        
       [simos12_4480060A0640977174100010.zip](https://github.com/user-attachments/files/25190034/simos12_4480060A0640977174100010.zip)
   
2.  **Read the password directly from the ECU** via PCMflash (I haven't tried it... PCMFlash may not be able to read the Simos12.1 password.)

      Read the password via PCMflash as specified in STEP1
* White 71 - This GPT 
* Brown 66 - This GPT
* Pink - I don't know
* Elow - I don't know
   
[TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf](https://github.com/user-attachments/files/25189430/TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf)
![boot2](https://github.com/user-attachments/assets/a397edf6-1623-44e3-b96e-1f16513de0a2)

> ⚠️ **Critical:** Without the correct password, flash access will be impossible. Ensure you have a valid backup.

---

## 🛠️ Step 2: Physical Preparation (Boot Mode)

1.  **Carefully peel back the lid fastener, heat it with a hair dryer to 100 degrees and slowly and carefully open it, cutting off the sealant with a knife**
![20260205_144207](https://github.com/user-attachments/assets/33072155-8155-49b4-b23c-6034d035b48a)

2.  **Connect as specified in the file STEP2**  [TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf](https://github.com/user-attachments/files/25189430/TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf) 
    *   boot1 grey wire to Grey wire pdf instructions, if the ECU does not give out identifiers, you can connect the boot pin to the ground via a 1000ohm resistor 
    ![20260205_200422](https://github.com/user-attachments/assets/1b9a96d2-97f3-4a93-8a18-17e7dc371047)

![20260205_200435](https://github.com/user-attachments/assets/a993b59a-6dba-4ac9-bbcd-447260bcf78d)
![20260205_200430](https://github.com/user-attachments/assets/ef513893-1e1f-4408-88f9-5fde6f8350bf)
![boot1](https://github.com/user-attachments/assets/16b07f34-0eaf-4e8e-a483-2ae7e51ecc44)

3.  Connect the Scanmatik2 adapter to the circuit.

---

## ⚡ Step 3: Flash Recovery via PCMflash

1.  Launch PCMflash and select the PCMflash 53 module Infineon: TC1797 MICRO (4096KB).
2.  Perform ECU identification 
![20260205_200514](https://github.com/user-attachments/assets/b4948bd4-41b4-43d9-9ffb-81df65f03ebc)

2.  Load your **Amt-bst** backup file **8V0906264E_iRom.Bin**.
3.  Load the password file **.pwd**  obtained in **Step 1**.
4.  Wait for the program to download 
![20260205_200445](https://github.com/user-attachments/assets/8b90aef1-57fa-400e-927a-97d4ee5b66a4)

5.  **Disconnect power** and **remove the resistors** after successful flash.

---

## ✅ Verification

*   Reconnect the ECU to the vehicle.
*   Attempt to establish a standard diagnostic connection.
*   If communication is restored, the recovery is successful.
*   Be sure to install the ECU cover on the sealant! 

## ⚠️ Warnings & Notes

*   This procedure carries a risk of permanent ECU damage.
*   Double-check all solder connections to avoid short circuits.
*   The exact resistor pins may vary. Consult the TC1797 datasheet.  
