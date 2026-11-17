# Lab 02: Crack a Specific User Account with Hydra

## Objective

Use THC Hydra's `http-post-form` module to test the supplied password list against the designated `securityadmin` account on the local LabEx practice website. Save the result to `hydra_results.txt`.

## Environment

- Platform: LabEx — Crack a Specific User Account
- Working directory: `~/project`
- Tool: THC Hydra
- Version shown: `9.2`
- Target: `localhost` (local practice environment)
- Web service: HTTP on TCP port `8080`
- Username: `securityadmin`
- Password list: `~/project/passwords.txt`
- Output file: `~/project/hydra_results.txt`

## Commands Executed

### 1. Move to the lab project directory

```bash
cd ~/project
```

- `cd`: changes the shell's current working directory.
- `~/project`: the project directory in the LabEx home environment.

The challenge requires Hydra to be executed from this directory.

### 2. Run Hydra against the practice website

```bash
hydra -l securityadmin -P passwords.txt localhost -s 8080 http-post-form "/:username=^USER^&password=^PASS^:Invalid username or password" -o hydra_results.txt
```

**Command breakdown:**

- `hydra`: launches THC Hydra.
- `-l securityadmin`: specifies the single username to test. Lowercase `-l` accepts one username.
- `-P passwords.txt`: supplies the password candidates from the file in the current project directory. Uppercase `-P` accepts a password list.
- `localhost`: sets the target to the local LabEx machine.
- `-s 8080`: selects TCP port 8080, where the practice website is running.
- `http-post-form`: selects Hydra's HTTP POST form authentication module.
- `"/:username=^USER^&password=^PASS^:Invalid username or password"`: defines the form request and failure condition:
  - `/`: login endpoint path.
  - `username`: username form field.
  - `password`: password form field.
  - `^USER^`: placeholder Hydra replaces with the specified username.
  - `^PASS^`: placeholder Hydra replaces with each candidate password.
  - `Invalid username or password`: failure text returned by the practice website when a login attempt is rejected.
- `-o hydra_results.txt`: writes Hydra's result output to the specified file.

**Important:** The form field names and failure message must match the target login form and its response. The values above are those used in this LabEx challenge.

### 3. Review the result file

The challenge specifies the output file at `~/project/hydra_results.txt`. It can be inspected from the project directory with:

```bash
cat hydra_results.txt
```

- `cat`: prints a file's contents in the terminal.
- `hydra_results.txt`: the output file supplied to Hydra with `-o`.

The screenshot shows Hydra reporting a valid login and the LabEx result checker confirming that the output file exists and contains the correct password. The full password is not transcribed here because the visible terminal line is clipped at the right edge of the screenshot.

## Understanding the Hydra Output

| Output | Meaning | Lab observation |
|---|---|---|
| `Hydra v9.2` | Version of Hydra being executed | Version 9.2 shown |
| `[DATA]` | Displays task and attempt information | Hydra began the HTTP POST form test |
| `attacking http-post-form://localhost:8080/` | Identifies the selected module, target, port, and endpoint | Local website on port 8080 |
| `Invalid username or password` | Failure response used to identify rejected credentials | Expected response for unsuccessful candidates |
| `[8080][http-post-form]` | Identifies the port and authentication module in the result | A valid result was reported |
| `login: securityadmin` | Username associated with the valid result | `securityadmin` |
| `password: ...` | Password Hydra identified | Password text is clipped in the screenshot |
| `1 of 1 target successfully completed` | Summary of target processing | One target completed |
| `1 valid password found` | Number of valid credentials reported | One valid password found |

The phrase `Invalid username or password` is the website's failed-login response configured in the module syntax. It is not an error indicating that Hydra itself failed.

## Screenshots

### Hydra HTTP POST form test and successful result

![Lab 02: Hydra testing the securityadmin account on localhost port 8080](Screenshot%20(767).png)

The screenshot shows the command executed from `~/project`, Hydra testing the `securityadmin` account against the local HTTP form, and the terminal reporting one valid password. The LabEx result checker also shows successful verification that the website is running and that `hydra_results.txt` exists and contains the correct password.

## Notes and Observations

- The exercise targets the local LabEx practice website, not an external host.
- The challenge supplied the password list at `~/project/passwords.txt`.
- The first displayed `Invalid username or password` response is the configured failed-login indicator.
- Hydra reported one valid password for the specified account.
- The LabEx checks confirmed that the website was running and the result file contained the correct password.
- The screenshot clips the password value at the terminal's right edge; therefore, this write-up does not guess or reproduce the complete credential.

## Key Learnings

- Use lowercase `-l` for one username and uppercase `-P` for a password file.
- Use `-s` to specify a non-default service port.
- Hydra's `http-post-form` module requires the endpoint, form field names, placeholders, and failure response.
- The failure string must match the website's actual unsuccessful-login response.
- Use `-o` to save results to a file for later review.
- A successful result is indicated by Hydra explicitly reporting a valid login/password pair; an attempted combination alone is not proof of success.

## Responsible Use

This exercise tests a deliberately provided account on the local LabEx practice website. Hydra generates authentication attempts and can lock accounts or disrupt services. Use it only in controlled labs or against systems for which you have explicit authorization. Never run password-guessing tests against third-party accounts or services without permission.
