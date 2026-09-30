# GOWIN FPGA Constraints Spreadsheet

A spreadsheet-based helper for building GOWIN physical (`.cst`) and timing (`.sdc`) constraint files for the **Sipeed Tang Nano 20K**.

It keeps the board pin information, clock definitions, and generated constraint text in one workbook. Rows can be enabled or disabled as needed, and the generated output is arranged for straightforward copy/paste into your project files.

## Versions

- [Google Sheets version](https://docs.google.com/spreadsheets/d/1TugTDtfr4J0ZP2S5aKtlnGsRTf4IM1rYJ8EbgRwCLTo/edit?usp=drivesdk)
- The Excel workbook can also be used directly with Microsoft Excel.

For the Google Sheets version, make your own copy before editing the master.

## Workflow

The tabs are arranged in working order:

1. **Instructions**
2. **Physical Constraints**
3. **Clock Constraints**
4. **CST Output**
5. **SDC Output**

### Physical Constraints

Enter or edit the HDL port name for each physical pin you want to use, set **Enabled** to `YES`, and choose the required I/O attributes.

Rows with **Enabled** set to `NO` are ignored when output is generated. This allows the workbook to retain useful board-pin information without emitting constraints for every listed pin.

The workbook generates `IO_LOC` and `IO_PORT` statements for enabled, named pins.

### Clock Constraints

Define primary and generated clocks.

Primary clocks generate `create_clock` statements. Generated clocks can include the source pin, master clock, and divide/multiply values needed for `create_generated_clock`.

For generated clocks:

- **Source Pin** is the pin feeding the generated clock element, such as a PLL input.
- **Port / Target Pin** is the generated clock output being constrained.

### CST Output

Open the **CST Output** tab and select the populated cells in column B from the first generated comment line through:

```text
// ******** end of generated file ********
```

Copy the selection and paste it into your `.cst` file.

### SDC Output

Do the same thing on the **SDC Output** tab to create the `.sdc` timing-constraint file.

The output is deliberately generated as **one line per spreadsheet cell**. Copying a single-column range produces newline-separated plain text in both Excel and Google Sheets, without requiring macros or scripts.

## Manual Constraints

The spreadsheet only generates the constraints represented by enabled rows.

Each generated file ends with:

```text
// ******** end of generated file ********
```

Put any hand-written constraints **after** that marker. When regenerating the file later, replace only the generated portion through the marker and leave your manual additions intact.

## Generated Output

The workbook currently generates:

- **Physical `.cst`** — `IO_LOC` and `IO_PORT` statements for enabled physical pins.
- **Timing `.sdc`** — `create_clock` and `create_generated_clock` statements for enabled clock definitions.

Custom constraints added after the end-of-generated-file marker are not generated or modified by the workbook.
