# Simos 12.1 RSA Off CBOOT Patch

This patch allows bypassing RSA signature verification on Simos 12.1 ECUs by enabling "Sample Mode" in CBOOT, which disables the cryptographic validation.

> **Disclaimer:** Based on reverse-engineering information from the [bri3d/VW_Flash](https://github.com/bri3d/VW_Flash) project for Simos 18. Use at your own risk.

---

## 📋 Target ECU Information

| Parameter           | Value                  |
| :------------------ | :--------------------- |
| **ECU Hardware**    | Simos12.1, 06K907425A  |
| **VW Part Number**  | 8V0906264E             |
| **VW ASW Version**  | 003                    |
| **Firmware**        | `FL_8V0906264E__0003`  |
| **BIN Software Code**        | `SC100CF0`             |

---


## 🛠️ How It Works

The patch modifies specific memory checks in the CBOOT to disable RSA verification. For technical details on the mechanism, see the original documentation:
*   [**VW_Flash Documentation**](https://github.com/bri3d/VW_Flash/blob/master/docs/docs.md)

### Assembly language

```
        Function		Memory Check	Required Value
                             RSA OFF 1 to 2  DA 00 3C 02 -> 00 00 00 00  (You don't have to use it)
                             LAB_80021b3e                                    XREF[1]:     80021b36(j)  
        80021b3e 91 80 fe 2a     movh.a     a2,#0xafe8
        80021b42 d9 22 a8 50     lea        a2=>DAT_afe80968,[a2]0x968
        80021b46 09 24 c0 08     ld.hu      d4,[a2]0x0=>DAT_afe80968
        80021b4a 6d ff ee f8     call       FUN_80020d26                                     undefined FUN_80020d26()
        80021b4e f6 23           jnz        d2,LAB_80021b54
        80021b50 da 00           mov        d15,#0x0
        80021b52 3c 02           j          LAB_80021b56
```

**Patch 1: Function at `0x8003230a`**
**Change:** `DA 00 3C 02` → `00 00 00 00`
```assembly

       Function		Memory Check	Required Value
                             RSA OFF 1 to 1  DA 00 3C 02 -> 00 00 00 00
                             LAB_8003230a                                    XREF[1]:     80032302(j)  
        8003230a 91 80 fe 2a     movh.a     a2,#0xafe8
        8003230e d9 22 a8 50     lea        a2=>DAT_afe80968,[a2]0x968
        80032312 09 24 c0 08     ld.hu      d4,[a2]0x0=>DAT_afe80968
        80032316 6d ff b7 a9     call       FUN_80027684                                     undefined FUN_80027684()
        8003231a f6 23           jnz        d2,LAB_80032320
        8003231c da 00           mov        d15,#0x0                                    // <-- Patch starts here
        8003231e 3c 02           j          LAB_80032322

```



### Patch Raw Bytes
Apply the patch via the Hex editor.
The checksum will need to be recalculated, many loaders calculate the checksum:

```
Hex adress    hex value 
21b50         DA 00 3C 02 DA 01 02 F2  ==> 00 00 00 00 DA 01 02 F2     // You don't have to use it.

3231c         DA 00 3C 02 DA 01 02 F2  ==> 00 00 00 00 DA 01 02 F2
```
The checksum can be calculated using a Python script. https://github.com/TheFlashBold/simos-12.1-stuff/blob/master/Patch%20ASW.md

## How to install it in the ECU 

<pre>
Simos12.1 can be read on the table without opening using Bench Mode

To read the ECU completely, including the OTP area, you will need - ECUbench-3.1.3.5 (AMTbst) 


1) Connect to the ECU and make a full backup of Irom and Erom (This is a full flash image including OTP)
2) Editing the Irom firmware via the Hex editor
3) Record the edited Irom in the ECU. The checksum will be recalculated during recording in the ECU.

That's it, the ECU can be installed in the car. 
Now you can record the modified FULL and CAL program via SimosTools or VW_Flash linux.

Note:
        Hardawre Version Number:H09 ==> X09  
        It doesn't change, we write the data directly to CBOOT.

</pre>

![20260117_172800](https://github.com/user-attachments/assets/b62f3d5b-8b7d-4c6b-9158-ee3210367c79)
![20250629_171723](https://github.com/user-attachments/assets/75de7e90-5023-404b-9396-03ed84de2e0f)


   Connecting Simos12 on the table   [VAG_Simos12.pdf](https://github.com/user-attachments/files/24864107/VAG_Simos12.pdf)





   
## How to install it in the ECU via SimosTools and VW_flash

<pre>
I think this is possible without connecting an ECU on the table, but I couldn't connect to the ECU via OpenPort2
and AMAleg (esp32-isotp-ble-bridge) on a Linux computer.
</pre>

