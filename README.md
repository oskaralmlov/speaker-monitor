# speaker-monitor

A lightweight script to keep your multiroom speaker group in sync — especially useful when using TV-connected speakers in combination with a LinkPlay-based multiroom setup.

---

### Why I Created This Project

I created **speaker-monitor** to solve a recurring issue with my **Audio Pro A26** speakers in a multiroom setup. I wanted the speakers to stay connected to my TV and also remain part of the multiroom group — without constant manual reconfiguration.

The problem was:

- If the A26 wasn’t the **master speaker**, switching its input (e.g. to TV/optical) would cause it to leave the group. I had to manually re-add it every time I wanted to listen to music across multiple rooms.
- If the A26 **was the master**, switching to the TV input would broadcast the TV sound to **every speaker** in the house — which wasn’t ideal.

This project eliminates those issues by keeping the speaker group intact and behaving as expected, even when inputs change.

---

### How It Works

`speaker-monitor` continuously monitors the group status and automatically re-adds speakers if they drop out unintentionally. Specifically, it:

1. Polls the **leader speaker** at set intervals to check the current group members.
2. For each defined **member speaker**, it:
   - Verifies whether it's still in the group.
   - If it's missing and idle, sets its volume and sends a join command to reconnect it to the group.

This ensures your multiroom setup stays in sync, without broadcasting unintended inputs or requiring manual intervention.

---

### Compatibility

This script has been tested with **Audio Pro A26** and **Audio Pro A10** speakers. It likely works with other **LinkPlay-enabled** speakers as well, but this hasn’t been confirmed. Your experience may vary depending on model and firmware.

---

### Installation

Clone the repository and install the required Python package:

```bash
pip install -r requirements.txt
```

Make the script executable and move it to a directory in your `PATH`:

```bash
chmod +x speaker-monitor
sudo cp speaker-monitor /usr/local/bin/
```

---

### Usage (Manual)

You can run the script manually with:

```bash
speaker-monitor \
  --leader-ip 192.168.1.100 \
  --member LivingRoom=192.168.1.110 \
  --member Kitchen=192.168.1.111
```

- Replace the IPs and speaker names with your actual setup.
- You can find the IP address of each speaker in the **Audio Pro app** under **Devices → Cogwheel → Speaker Information**.

---

### Running as a Service

To run `speaker-monitor` automatically in the background, use the provided `speaker-monitor.service` systemd unit file.

> ⚠️ **Note:** This setup assumes your speakers have **stable IP addresses** — either configured as static IPs or assigned via **static DHCP leases**. The IPs must remain consistent over time for the script to function correctly.

1. Copy the service file to the system's unit directory:
   ```bash
   sudo cp speaker-monitor.service /etc/systemd/system/
   ```

2. Edit the file to update the `--leader-ip` and `--member` values with the correct IP addresses (you can find these in the Audio Pro app).

3. Enable and start the service:
   ```bash
   sudo systemctl enable --now speaker-monitor.service
   ```

The script will now run continuously in the background and start automatically on boot, keeping your speaker group synchronized.

---

### Requirements

This script requires Python 3 and the following Python package:

```
requests
```

Install it with:

```bash
pip install -r requirements.txt
```

---

