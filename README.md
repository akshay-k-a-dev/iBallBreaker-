# RTL8192EU Driver for Linux 🚀

## The Problem 😢
So you got yourself an **iBall Baton 300M USB Wi-Fi Adapter**, plugged it into your Linux machine, and... nothing. No Wi-Fi. No love. Meanwhile, Windows users are out there streaming, gaming, and laughing at us Linux folks. Why? Because **the driver is only available for Windows!**

But fear not! I did what any sane person would do—**reverse-engineered the Windows `driver.exe` file**, dug through its secrets like a true hacker, and found the appropriate driver. Now, here’s how you can get this thing working on Linux too! 💪🐧

---

## The Solution 🛠️
We’re going to manually download, compile, and install the correct driver so Linux finally recognizes this Wi-Fi dongle. Let’s make Wi-Fi work again! 🔥

---

## Installation Steps

### **Step 1: Install Required Dependencies**
First, let’s install the tools we need to build the driver:
```bash
sudo apt update
sudo apt install build-essential dkms git linux-headers-$(uname -r)
```

### **Step 2: Download and Compile the Driver**
1. Clone the driver repository like a pro:
   ```bash
   git clone https://github.com/Mange/rtl8192eu-linux-driver.git
   cd rtl8192eu-linux-driver
   ```
2. Compile and install the driver:
   ```bash
   make
   sudo make install
   ```
3. Load the driver into your system:
   ```bash
   sudo modprobe 8192eu
   ```

### **Step 3: Verify Wi-Fi Adapter**
Time to check if it’s working:
```bash
ip link show
iwconfig
```
If you see your Wi-Fi network appear, **congratulations!** You just defeated Windows-exclusive hardware! 🎉

### **Step 4: Make the Driver Load on Boot**
We don’t want to do this every time we restart, so let’s automate it:
```bash
echo "8192eu" | sudo tee -a /etc/modules
```
Now reboot:
```bash
sudo reboot
```

### **Step 5: Eliminate Conflicting Modules (If Wi-Fi Doesn’t Work)**
Sometimes, Linux loads the wrong Realtek driver. Let’s check:
```bash
lsmod | grep rtl
```
If you see `r8188eu` or `rtl8xxxu`, blacklist them:
```bash
echo "blacklist r8188eu" | sudo tee -a /etc/modprobe.d/blacklist.conf
echo "blacklist rtl8xxxu" | sudo tee -a /etc/modprobe.d/blacklist.conf
```
Reboot again for good measure.

---

## **Uninstallation (If You Want to Rage Quit)**
If something breaks or you just don’t like it anymore, remove the driver with:
```bash
cd rtl8192eu-linux-driver
sudo make uninstall
```

## **Troubleshooting (Because Nothing Ever Works on the First Try)**
- If `make` fails, try installing the correct **kernel headers**:
  ```bash
  sudo apt install linux-headers-$(uname -r)
  ```
- If the adapter isn’t detected, try manually reloading the driver:
  ```bash
  sudo modprobe -r 8192eu
  sudo modprobe 8192eu
  ```

---

## **References (Because I Didn’t Just Make This Up)**
- [GitHub Repo: rtl8192eu-linux-driver](https://github.com/Mange/rtl8192eu-linux-driver)
- [Realtek Official Drivers](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-wireless)

---

## **Final Thoughts**
This guide should help you get your **iBall Baton 300M Wi-Fi Adapter** working on Linux. If you found it useful, star this repo ⭐, share it with your Linux friends, and show Windows who’s boss! 💻🔥

