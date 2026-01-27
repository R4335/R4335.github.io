# The Fortress and the clerk - Intro to the linux kernel - Part 1
### Why your computer is actually to computers, why everything is a file and why the kernel is the ultimate bureaucrat.

### The great divide - user space vs kernel space
Imagine your computer is a medieval kingdom.
- **The Citizens (user space/Ring 3):** This is where your web browser, your music player, and your text editor live. They are free to do whatever they want within their own homes, but they have zero power over the kingdom itself. They cannot touch the hardware directly.
- **The King (kernel space/Ring 0):** This is the Linux kernel. It has the absolute power. It controls the memory (land), the CPU (labor) and the devices (resources).

### Protection Rings
The CPU enforces this separation using "protection rings".
- **Ring 0:** Full access to everything. If code crashes here, the whole machine dies (*kernel panic*).
- **Ring 3:** Restricted access. If code crashes here, only that program dies.
 
### Check your privilege
You can actually see this separation in action. Run the following command in your terminal.
```
id
```

You will see a *uid (User ID)*. Unless that ID is *0 (root)*, you are a peasant. You are stuck in Ring 3. Even *root* is techinically a user space concept, the kernel itself is above even root. The kernel doesn't have UID. It is the system.

***Will Continue part 2 soon...***
