# Lab 02: Save Nmap Scan Output to XML

## Objective

Run a scan of local TCP port 8080 and save the results in Nmap's XML output format for later inspection or use by other tools.

## Environment

- Platform: LabEx Nmap for Beginners lab
- Working directory: `/home/labex/project`
- Target: `localhost` (`127.0.0.1`)
- Target port: `8080/tcp`
- Nmap version shown: `7.80`
- Output file: `scan_report.xml`

## Commands Attempted

The terminal screenshot shows several unsuccessful command-line attempts before the working command. They produced the Nmap message:

```text
Output filename begins with '-'. Try '-oX ./-p' if you really want it to be named as such.
QUITTING!
```

The issue was the placement of the XML output option and its filename. Nmap's `-oX` option expects the XML output filename immediately after it. In the unsuccessful attempts, `-p` was placed where Nmap expected the filename, so Nmap interpreted an option-like string as the output filename and stopped.

The shell redirection operator `>` is not needed when Nmap's own `-oX` output option is used.

## Successful Command

```bash
nmap -oX scan_report.xml -p 8080 localhost
```

### Explanation of each part

- `nmap`: starts Nmap.
- `-oX scan_report.xml`: writes the scan results in XML format to a file named `scan_report.xml`.
- `-p 8080`: scans only TCP port 8080.
- `localhost`: sets the target to the local machine.

**Important:** Keep the output filename directly after `-oX`. Put other scan options, such as `-p 8080`, elsewhere in the command.

## Observed Result

The successful command completed and reported:

- Target: `localhost (127.0.0.1)`
- Host status: up
- Port: `8080/tcp`
- Port state: `open`
- Service label: `http-proxy`
- Scan summary: one IP address scanned

The LabEx result checker confirmed:
- XML file exists.
- Nmap ran successfully.
- Port 8080 was scanned.

## Inspect the XML File

The lab instructions indicate that the generated file can be inspected with:

```bash
cat scan_report.xml
```

- `cat` prints a file's contents to the terminal.
- `scan_report.xml` is the XML report created by the successful Nmap command.

The screenshot shows the file-creation check passing; it does not show the output of the `cat` command, so the XML contents are not reproduced here.

## Why XML Output Is Useful

Nmap XML output is structured data. Compared with normal terminal output, XML is easier for scripts and other security tools to parse, transform, store, and integrate into reporting workflows.

## Key Learnings

- Use `-oX <filename>` to save Nmap results as XML.
- The filename must immediately follow `-oX`.
- Do not confuse Nmap's `-oX` output handling with shell output redirection (`>`).
- Validate that the report file was created after the scan.
- Use `cat scan_report.xml` to inspect the generated file in the terminal.

## Responsible Use

This exercise scanned `localhost` in a controlled lab. Scan only systems you own or have explicit authorization to test.
