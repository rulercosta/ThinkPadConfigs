# ThinkFan configuration

## Attention! Use at your own risk.

### 1. Install necessary package(s)

For Arch based distros

    yay -S thinkfan


For Debian based distros

    sudo apt install thinkfan

### 2. Set-up the configuration file

    cd ~ && git clone git@github.com:rulercosta/configs.git
    sudo sh -c 'cp configs/ThinkFan/thinkfan.conf /etc/thinkfan.conf'

Contents of `/etc/thinkfan.conf` config file is as follows

    fan information
    temperature information
    (speed [0 - 7], temp lower level, temp upper level)

Fan sensor

    tp_fan /proc/acpi/ibm/fan

Temperature sensors

    find /sys/devices -type f -name 'temp*_input'

Append `hwmon` keyword in front of each line of the output as follows

    hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp6_input
    hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp3_input
    hwmon /sys/devices/platform/thinkpad_hwmon/hwmon/hwmon4/temp7_input
    ...

Fan-Temp config `(speed level , start temp , end temp)`

    (0, 0,  55)
    (1, 48, 60)
    (2, 50, 61)
    (3, 52, 63)
    (4, 56, 65)
    (5, 59, 66)
    (7, 63, 32767)

### 3. Enable acpi_fancontrol kernal module (disabled by default)

    echo 'options thinkpad_acpi fan_control=1' >> /etc/modprobe.d/thinkpad_acpi.conf
    sudo modprobe -r thinkpad_acpi && sudo modprobe thinkpad_acpi

### 4. Enable thinkfan service

Enable ThinkFan service using systemd

    sudo systemctl enable --now thinkfan.service
    
Check service status

    sudo systemctl status thinkfan.service

## If anything went wrong, don't @ me, curse your own fate :p
