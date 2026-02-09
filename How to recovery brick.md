# How to Recover Bricked Simos12.1 ECU

**Problem:** Firmware experiments led to a crash of CBOOT. The ECU no longer responds via GPT.

---

## 📋 Prerequisites & Tools

**We will need:**

### Hardware
*   Scanmatik 2
*   3-4 resistors of 1000 ohm

### Software & Data
*   PCMflash 53 module Infineon: TC1797 MICRO (4096KB)
*   **Full backup `amt-bst`** (you will need a password to access the flash)

---

## 🔑 Step 1: Reading the Password

There are 2 ways to read the password:

1.  **Create a `.pwd` file** (Method details...)
2.  **Read the password directly from the ECU** via PCMflash (Method details...)

   1)Пароль для PCMFlash  находится в OTP Full Backup  по адресу 1420c - 1421b пример 10 7F F4 0F B0 A7 BE 72 06 DA 06 01 6C 28 35 BB 14

   chip id 14200-1420B    пример 44 80 06 0A 06 40 97 71 74 10 00 10

   Создать файл с расширением .pwd и поместить туда через hexeditor 10 7F F4 0F B0 A7 BE 72 06 DA 06 01 6C 28 35 BB 14

2) Прочитать пароль через PCMflash как указанно в STEP1 

[TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf](https://github.com/user-attachments/files/25189430/TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf)



> ⚠️ **Critical:** Without the correct password, flash access will be impossible. Ensure you have a valid backup.

---

## 🛠️ Step 2: Physical Preparation (Boot Mode)

1.  Аккуратно отогнуть крепление крышки, прогреть феном до 100 градусов  и медленно и аккуратно открывать, срезая герметико ножом 
2.  Подключить  как  указанно в файле [TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf](https://github.com/user-attachments/files/25189430/TRICORE_VAG_SIMOS12_TC1797_INTFLASH.pdf) 
    *   boot1 серый провод к Grey wire инструкции pdf, если ЭБУ не отдает индетефикаторы можно подключить boot pin на массу через резистор 1000ом 
   
3.  Connect the Scanmatik 2 adapter to the circuit.

---

## ⚡ Step 3: Flash Recovery via PCMflash

1.  Launch PCMflash and select the PCMflash 53 module Infineon: TC1797 MICRO (4096KB).
2.  Выполнить идентификацию блока 
2.  Load your `amt-bst` backup file 8V0906264E_iRom.Bin.
3.  Load the password file **.pwd**  obtained in **Step 1**.
4.  Дождитесь загрузки программы 
5.  **Disconnect power** and **remove the resistors** after successful flash.

---

## ✅ Verification

*   Reconnect the ECU to the vehicle.
*   Attempt to establish a standard diagnostic connection.
*   If communication is restored, the recovery is successful.

## ⚠️ Warnings & Notes

*   This procedure carries a risk of permanent ECU damage.
*   Double-check all solder connections to avoid short circuits.
*   The exact resistor pins may vary. Consult the TC1797 datasheet.  
