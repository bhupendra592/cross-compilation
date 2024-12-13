# Cross-Compiling a C Program for Raspberry Pi on Ubuntu 20.04

This guide explains the process of cross-compiling a C program for a Raspberry Pi 3 Model B+ on Ubuntu 20.04. Cross-compilation is compiling code on one platform to run on a different platform, such as compiling ARM binaries on an x86 machine.

## **Steps to Perform on Ubuntu 20.04**

### **Step 1: Install the Cross-Compiler**

Install the appropriate cross-compilation toolchain for the Raspberry Pi. The toolchain `gcc-arm-linux-gnueabihf` is used for generating ARM-compatible executables.

```bash
sudo apt update
sudo apt install gcc-arm-linux-gnueabihf
```

- **gcc-arm-linux-gnueabihf**: A cross-compiler for ARM architecture with hardware floating-point support.

### **Step 2: Write a Sample Program in C**

Create a sample C program. For this example, we will write a program named `HelloIoT.c`.

```bash
nano HelloIoT.c
```

Add the following code:

```c
#include <stdio.h>

int main() {
    printf("Hello, IoT!\n");
    return 0;
}
```

Save the file and exit the editor.

### **Step 3: Cross-Compile the Program**

Use the cross-compiler to generate an executable for the Raspberry Pi:

```bash
arm-linux-gnueabihf-gcc -o HelloIoT HelloIoT.c
```

### **Step 4: Verify the Executable**

Check that the executable file has been created:

```bash
ls
```
Output:
```
HelloIoT  HelloIoT.c
```

The executable `HelloIoT` is now ready to run on the Raspberry Pi.

### **Step 5: Transfer the Executable to Raspberry Pi**

Use `scp` to copy the executable to the Raspberry Pi. Replace `username` with your Raspberry Pi’s username and `ipaddress` with its IP address.

```bash
scp HelloIoT username@ipaddress:/home/pi
```

### **Step 6: Run the Executable on Raspberry Pi**

1. Log in to the Raspberry Pi:

   ```bash
   ssh username@ipaddress
   ```

2. Navigate to the directory where the executable is located:

   ```bash
   cd /home/pi
   ```

3. Make the executable runnable (if required):

   ```bash
   chmod +x HelloIoT
   ```

4. Run the program:

   ```bash
   ./HelloIoT
   ```

   Output:
   ```
   Hello, IoT!
   ```

### **Example Terminal Outputs**

#### **Host Machine (Ubuntu)**

```bash
$ nano HelloIoT.c
$ arm-linux-gnueabihf-gcc -o HelloIoT HelloIoT.c
$ ls
HelloIoT  HelloIoT.c
$ file HelloIoT
HelloIoT: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.4.0, not stripped
$ scp HelloIoT pi@192.168.1.10:/home/pi
```

#### **Raspberry Pi**

```bash
$ ssh pi@192.168.1.10
$ cd /home/pi
$ chmod +x HelloIoT
$ ./HelloIoT
Hello, IoT!
```

## **Understanding the Cross-Compilation Process**

Cross-compilation involves the following:

- **Compiler**: The cross-compiler `gcc-arm-linux-gnueabihf` generates ARM binaries on an x86 host machine.
- **ELF File**: The output `HelloIoT` is an ELF (Executable and Linkable Format) binary for ARM.
- **Target Architecture**: The Raspberry Pi 3 Model B+ uses an ARM Cortex-A53 CPU, which is a 32-bit or 64-bit ARMv8-A architecture.

## **Verification of Target Architecture**

To ensure the executable is built for ARM:

1. Use the `file` command on Ubuntu:

   ```bash
   file HelloIoT
   ```
   Output:
   ```
   HelloIoT: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.4.0, not stripped
   ```

2. Use `readelf` for more details:

   ```bash
   readelf -h HelloIoT
   ```
   Key fields:
   - **Class**: `ELF32`
   - **Machine**: `ARM`

## **Conclusion**

You have successfully cross-compiled a simple "Hello, IoT!" program on Ubuntu 20.04 for a Raspberry Pi 3 Model B+. By following these steps, you can develop applications on your host machine and run them seamlessly on your Raspberry Pi.


