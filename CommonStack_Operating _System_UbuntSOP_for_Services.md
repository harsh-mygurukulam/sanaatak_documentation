
# 🧾 SOP: Managing Services on Ubuntu

**Common Stack** | **Operating System** | **Ubuntu**  
**Component:** System Services (Using `systemctl`)  



## 📝 Description

This SOP provides step-by-step instructions to start, stop, enable, disable, and check the status of services on Ubuntu-based systems using the `systemctl` command. It is written for users who may have little or no background in Linux system administration. Mastering these service management commands is essential to ensure the proper operation and maintenance of services such as web servers, databases, and schedulers.



## ✅ Acceptance Criteria

This SOP must include and clearly explain:
- How to **start** a service using `systemctl`
- How to **stop** a service
- How to **restart** a service
- How to **enable** a service to start automatically on system boot
- How to **disable** a service from starting on boot
- How to **check the status** of a service
- Each section should have command examples with clear instructions


## 🔧 Systemctl Commands Explained

### ▶️ 1. Start a Service

Starts the service immediately without reboot.

```bash
sudo systemctl start <service_name>
```

_Example:_

```bash
sudo systemctl start nginx
```

---

### ⏹ 2. Stop a Service

Stops a running service.

```bash
sudo systemctl stop <service_name>
```

_Example:_

```bash
sudo systemctl stop nginx
```

---

### 🔁 3. Restart a Service

Restarts the service. Useful when configuration changes have been made.

```bash
sudo systemctl restart <service_name>
```

_Example:_

```bash
sudo systemctl restart nginx
```

---

### 🔍 4. Check Service Status

Displays the current state (running, stopped, failed) of a service.

```bash
sudo systemctl status <service_name>
```

_Example:_

```bash
sudo systemctl status nginx
```

---

### 🚀 5. Enable a Service (Start at Boot)

Ensures the service starts automatically at every system boot.

```bash
sudo systemctl enable <service_name>
```

_Example:_

```bash
sudo systemctl enable nginx
```

---

### 🚫 6. Disable a Service (Don't Start at Boot)

Prevents the service from starting automatically on boot.

```bash
sudo systemctl disable <service_name>
```

_Example:_

```bash
sudo systemctl disable nginx
```

---

## 💡 Example: Apache2 Web Server Management

Here’s a real-world example with Apache2:

```bash
sudo systemctl start apache2         # Start the service
sudo systemctl enable apache2        # Enable on boot
sudo systemctl status apache2        # Check current status
sudo systemctl restart apache2       # Restart if needed
sudo systemctl stop apache2          # Stop the service
sudo systemctl disable apache2       # Disable on boot
```
**End Of SOP**
