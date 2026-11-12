# Lab 01: Install and Verify Hydra

## Objective

Install THC Hydra, verify that it is available in the terminal, and perform a basic controlled test against the SSH service running on the local LabEx environment.

## Environment

- Platform: LabEx — Install and Verify Hydra
- Working directory: `~/project`
- Tool: THC Hydra
- Version shown: `9.2`
- Target: `127.0.0.1` (localhost)
- Service: SSH on TCP port `22`

## Commands Executed

### 1. Install Hydra

```bash
sudo apt install -y hydra
```

Command breakdown:

- `sudo`: runs the command with elevated privileges.
- `apt`: Debian/Ubuntu package manager.
- `install`: installs the requested package.
- `-y`: automatically confirms the package manager's installation prompt.
- `hydra`: package name for THC Hydra.

**Observed result:** the terminal reported that Hydra was already the newest version, `9.2-1ubuntu1`. It also reported that no new packages were installed during this run.

### 2. Verify the Hydra executable location

```bash
which hydra
```

- `which`: displays the path of the executable that would run when the command is entered.
- `hydra`: the executable being located.

**Observed result:**

```text
/usr/bin/hydra
```

This confirms that the Hydra executable is available through the system's command path.

### 3. Run a controlled SSH password-list test

The screenshot shows the following command being run against the local lab host:

```bash
hydra -L ~/project/test_usernames.txt -P ~/project/test_passwords.txt 127.0.0.1 ssh -t 1 -vV
```

Command breakdown:

- `hydra`: launches THC Hydra.
- `-L ~/project/test_usernames.txt`: supplies a file containing the candidate usernames. Uppercase `-L` means a username list.
- `-P ~/project/test_passwords.txt`: supplies a file containing candidate passwords. Uppercase `-P` means a password list.
- `127.0.0.1`: target IP address; this is the loopback address for the local machine.
- `ssh`: selects SSH as the authentication service to test.
- `-t 1`: limits Hydra to one parallel task for this target.
- `-vV`: enables verbose output and displays each login/password pair being attempted.

**Observed result:** Hydra v9.2 started and reported that it was testing SSH password authentication on `127.0.0.1:22`. The output showed 9 login attempts based on 3 usernames and 3 passwords, and displayed an attempt using username `admin` and password `password`.

The screenshot shows the test in progress. It does not show a successful authentication result, so this write-up does not claim that a password was discovered or that login succeeded.

## Understanding the Output

| Output | Meaning |
|---|---|
| `Hydra v9.2` | Version of Hydra being executed |
| `[DATA]` | Displays task and attempt information |
| `attacking ssh://127.0.0.1:22/` | Identifies the SSH service and target port |
| `[INFO]` | Reports information about the service, including password-authentication support |
| `[ATTEMPT]` | Shows a username/password combination being tested |
| `-t 1` | Runs one task at a time for the target |

## Screenshots

### Hydra installation and verification

![Hydra installation using apt and executable path verification](Screenshot%20(751).png)

This screenshot shows the `sudo apt install -y hydra` command, the package manager confirming Hydra is already installed, and `which hydra` returning `/usr/bin/hydra`.

### Basic SSH password-list test

![Hydra SSH test against localhost](Screenshot%20(752).png)

This screenshot shows Hydra v9.2 testing the SSH service on `127.0.0.1:22`, with verbose output displaying the attempted username/password combinations. The visible output is an attempt in progress, not proof of a successful login.

## Key Learnings

- Hydra can be installed through the Ubuntu/Debian package manager.
- `which hydra` verifies that the executable is available and shows its path.
- Hydra accepts username and password lists through `-L` and `-P`.
- The service name identifies the authentication protocol being tested.
- Verbose output helps observe the test process.
- A test attempt is not evidence of a successful login; only an explicit success result would establish that.

## Responsible Use

This exercise targets the local LabEx environment at `127.0.0.1`. Authentication testing can affect accounts and services. Run Hydra only against systems you own or have explicit authorization to test, and use test accounts and lab credentials.
