# 🧟‍♂️ ZombieLoad PoC

This repository contains two applications to demonstrate **ZombieLoad as an example of Microarchitectural Data Sampling (MDS)**.
For technical information about the bug, refer to the paper:

📄 *[ZombieLoad: Cross-Privilege-Boundary Data Sampling](https://zombieload.com/zombieload.pdf)*
by Schwarz, Lipp, Moghimi, Van Bulck, Stecklina, Prescher, and Gruss

---

## 🧪 Proof of Concept (PoC)

This repository contains a **Proof of Concept attack** showing ZombieLoad on **Windows 10**.
It also includes a **victim application** to test the leakage in various scenarios.

🧠 This demo was tested with an Intel Core **i7-7700k**, but it should work on any Windows 10 system with a modern Intel Core or Xeon CPU (2010 or newer).

⚡ For best results, use a fast CPU that supports **Intel TSX** (e.g. most i7-5xxx, i7-6xxx, or i7-7xxx).

---

## 🛠️ Building

The PoCs only require **MinGW-w64** to compile.
Building the attacker or victim is as simple as running:

```bash
make
```

in the respective application folder.

📦 *Alternatively, you can try out the precompiled executables in the `v1.0` release.*

---

## ▶️ Run

### 🕵️ Attacker Application

This variant does **not** require special CPU features or privileges.
Run the attacker on the first hyperthread (affinity mask: `0b1`):

```cmd
start /affinity 1 .\leak.exe
```

🕑 It may take a while until the leakage starts. Launching memory-intensive apps (e.g., a browser) can help reduce this delay.

---

### 🎯 Victim Application

Run the victim on the **same physical core**, but a **different hyperthread** (mask: `0b10000`):

```cmd
start /affinity 16 .\secret.exe
```

You can also pass a **secret letter** as a parameter:

```cmd
start /affinity 16 .\secret.exe M
```

By default, the secret letter is `'A'`.
As soon as the victim starts, the attacker should show a **clear signal** — the bar for the leaked letter will grow.

---

## 🆘 Help

### 🧩 How do I know which core IDs are hyperthreads?

Use the [Coreinfo tool from Windows Sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/coreinfo).
🧮 The core count for affinity masks starts at `0b1`.

---

### 💻 Can I run the PoC in a virtual machine?

Yes — it **works in VMs**, though it may perform worse due to virtualization overhead.

---

### 🤔 It just doesn't work on my computer — what can I do?

There are many possible causes. Try the following tips:

* 🔋 Make sure your CPU frequency is **at maximum** and that **frequency scaling is disabled**
* 🔌 If on a laptop, plug it in for **maximum performance**
* 🎯 Pin the attacker and victim to specific cores (e.g. with `taskset`) — both must be on the **same physical core**
* 🌀 Try changing system load — more or less background activity can make a difference
* 🔄 Restart the demos — or even your computer. After standby, **timing issues** can occur on some systems.
