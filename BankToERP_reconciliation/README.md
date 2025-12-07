Bank to ERP Reconciliation Robot (UiPath Demo)

A compact UiPath demo robot that reconciles bank statement transactions against ERP payment data using simple deterministic matching (Date + Amount).
Designed to demonstrate clean workflow structure, correct argument mapping, reusable logging, and clear execution output.

Overview

The robot performs:

Loads configuration from GlobalConfig.xlsx.

Reads two CSV inputs:

Bank_Statement.csv

ERP_Payments.csv

Reconciles transactions:

Matches on Date and Amount

Builds Matched, Bank-Unmatched, and ERP-Unmatched tables

Writes output to:

Data/Output/BankReconciliation_Output.xlsx

Prints a concise run summary in the Output panel.

Runs end-to-end in ~1 second.

Project Structure
BankToERP_Reconciliation/
│
├── Data/
│   ├── Input/
│   │   ├── Bank_Statement.csv
│   │   └── ERP_Payments.csv
│   ├── Output/
│   │   └── BankReconciliation_Output.xlsx
│   └── GlobalConfig.xlsx
│
├── Workflows/
│   ├── Init/
│   │   ├── Init_LoadConfig.xaml
│   │   └── Init_LoadInputFiles.xaml
│   ├── Process/
│   │   └── Process_MatchTransactions.xaml
│   └── Close/
│       └── Close_GenerateOutputFiles.xaml
│
├── Helpers/
│   └── Helper_Logging.xaml
│
└── Main_BankToERP.xaml

Workflow Responsibilities
Main_BankToERP.xaml

Orchestration workflow.

Logs process start/end using Helper_Logging.xaml.

Loads configuration + input tables.

Invokes Process and Close workflows.

Produces final Excel output.

Prints summary (Bank rows, ERP rows, matched/unmatched counts).

Tables stored here:

BankTable_Main

ERPTable_Main

MatchedTable_Main

BankUnmatched_Main

ERPUnmatched_Main

Init_LoadConfig.xaml

Reads GlobalConfig.xlsx and exposes values via Out arguments.

Init_LoadInputFiles.xaml

Reads and parses:

Bank_Statement.csv → out_BankTable

ERP_Payments.csv → out_ERPTable

Both include header rows.

Process_MatchTransactions.xaml

Core logic.

Arguments:

in_BankTable, in_ERPTable

out_MatchedTable, out_BankUnmatchedTable, out_ERPUnmatchedTable (In/Out)

Logic:

For each Bank row:

Match ERP by Date & Amount

Add to Matched or Bank-Unmatched

After Bank pass:

Add unmatched ERP rows to ERP-Unmatched

Row-level logs exist but are disabled for demo clarity (annotated).

Close_GenerateOutputFiles.xaml

Builds final Excel workbook with three sheets:

Matched

Bank_Unmatched

ERP_Unmatched

Helper_Logging.xaml

Small reusable logger.

Arguments:

in_Source

in_Message

in_Level (default: Info)

Used only for high-level start/end messages.

Example Output (UiPath Output Panel)
[Main] BankToERP_Reconciliation execution started
Starting the Bank to ERP reconciliation demo.
Loaded 5 configuration settings.
Bank rows loaded: 20
ERP rows loaded: 20
Matched table initialized.
Bank unmatched table initialized.
ERP unmatched table initialized.
Beginning reconciliation pass...
Completed matching pass for all Bank rows. Total matched: 19, Total unmatched (Bank): 1
Completed unmatched check for ERP rows. Total unmatched (ERP): 1
Writing reconciliation output to Data/Output/BankReconciliation_Output.xlsx
Reconciliation output file generated successfully.
----------------------------------------
Summary:
- Bank total rows: 20
- ERP total rows: 20
- Matched: 19
- Bank unmatched: 1
- ERP unmatched: 1
[Main] Reconciliation process completed
Reconciliation demo completed.

Screenshots
Main workflow

Matching logic (top)

Matching logic (bottom)

Execution log

Excel output

Project folder structure

Disabled debug logs (optional)

Notes

This is a demo robot, not a production implementation.

Matching logic is intentionally simple.

Row-level logs remain in the workflow but are disabled for clarity.

The project demonstrates structured workflow design and clean argument handling.

License

This project is licensed under the MIT License