# Lab 01: Nmap Scanning and Output Analysis

## Objective

The objective of this lab is to understand basics of Nmap and perform a basic Nmap scan against the local test environment, inspect the result, and understand the fields Nmap reports for a discovered TCP port.

## Environment

- Platform: LabEx Nmap for Beginners lab
- Working directory: `/home/labex/project`
- Target: `localhost` (resolved by Nmap to `127.0.0.1`)
- Target port: `8080/tcp`
- Nmap version shown: `7.80`

The screenshots show a web server listening on port 8080 in a separate terminal. The command used to start that server is not visible in the screenshots, so it is not reproduced here.

## Commands Executed

### 1. Move to the lab project directory

```bash
cd /home/labex/project
```

Changes the shell's current working directory to the lab project folder. This is where the test page and other lab files are stored.

### 2. Scan TCP port 8080 on localhost

```bash
nmap -p 8080 localhost
```

- `nmap`: launches the Nmap network scanner.
- `-p 8080`: limits the scan to TCP port 8080 instead of scanning the default port set.
- `localhost`: scans the local machine (shown as `127.0.0.1`).

**Observed result:** the host was up and `8080/tcp` was reported as `open`, with the service label `http-proxy`. The scan reported one IP address scanned.

### 3. Create a simple test web page

```bash
echo "Welcome to Nmap Testing Environment" > index.html
```

- `echo` prints the supplied text.
- `>` redirects that text into a file.
- `index.html` is created in the current directory (or overwritten if it already exists).

This creates sample content for the HTTP service in the lab.

### 4. Request the local web page

```bash
curl http://localhost:8080
```

- `curl` makes an HTTP request to the specified URL.
- `localhost` identifies the local machine.
- `:8080` selects port 8080.

**Observed result:** the response displayed:

```text
Welcome to Nmap Testing Environment
```

This confirms that the local HTTP endpoint returned the test page content.

### 5. Repeat the port scan

```bash
nmap -p 8080 localhost
```

The scan was repeated after creating and requesting the test page. The observed result again showed `8080/tcp open http-proxy`.

## Understanding the Nmap Output

| Output field | Meaning | Lab observation |
|---|---|---|
| `Nmap scan report` | Identifies the scan target | `localhost (127.0.0.1)` |
| `Host is up` | The target responded to Nmap's host check | Host was reachable |
| `PORT` | Port number and transport protocol | `8080/tcp` |
| `STATE` | Nmap's assessment of the port state | `open` |
| `SERVICE` | Nmap's service-name identification | `http-proxy` |
| `Nmap done` | Scan summary | One IP address scanned |

The `http-proxy` label is Nmap's service identification for the port; it does not by itself prove the exact application or proxy software running there.

## Notes and Observations

- The target was the loopback host, not an external system.
- Port 8080 was reachable and reported open during the scans.
- The HTTP request returned the test page content.
- A separate server terminal showed an HTTP server listening on port 8080. A request-handler connection-reset traceback also appeared in that terminal; it was not the successful `curl` response shown in the other terminal.

## Key Learnings

- Use `-p` to focus a scan on a specific port.
- An `open` port indicates that a service is accepting TCP connections.
- Nmap's service column is an identification, not a guarantee of the exact application.
- `curl` can be used to verify HTTP behavior after discovering a web-facing port.
- Normal Nmap output is useful for quick, human-readable inspection.

## Responsible Use

This exercise targeted `localhost` in a controlled lab. Scan only systems you own or have explicit authorization to test.
