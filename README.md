## Useful command lines in Linux (Ubuntu)

### Delete all empty files

```bash
find . -type f -size 0 -exec rm -f '{}' +
```

### Get first column in formatted data file

```bash
awk -F " " '{print $1}' data.txt
```

### Get lines from line 10 to line 20

```bash
head -n 20 data.txt | tail -n 10
```

### Change Display Resolution mode

```bash
cvt 1920 1080 60
```


Output looks like this. Copy the modeline after Modeline to use it in the next command:

```
# 1920x1080 59.96 Hz (CVT 2.07M9) hsync: 67.16 kHz; pclk: 173.00 MHz
Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
```

Create a new mode with the copied modeline and xrandr

```bash
xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync

xrandr --query | grep connected
```

The output looks like this.

```
Virtual1 connected primary 1360x768+0+0 (normal left inverted right x axis y axis) 0mm x 0mm
Virtual2 disconnected (normal left inverted right x axis y axis)
Virtual3 disconnected (normal left inverted right x axis y axis)
Virtual4 disconnected (normal left inverted right x axis y axis)
Virtual5 disconnected (normal left inverted right x axis y axis)
Virtual6 disconnected (normal left inverted right x axis y axis)
Virtual7 disconnected (normal left inverted right x axis y axis)
Virtual8 disconnected (normal left inverted right x axis y axis)
```

Then Add new mode and change the monitor resolution:

```bash
xrandr --addmode Virtual1 "1920x1080_60.00"
xrandr --output Virtual1 --mode "1920x1080_60.00"
```

### Add bash for one user

```bash
sudo chsh -s /bin/bash USER_NAME
```
