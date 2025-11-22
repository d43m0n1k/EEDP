# EEDP
EEDP is an interactive PowerShell script that:

Pulls users from Entra ID (Azure AD) via Microsoft Graph

Merges standard Entra fields with PANL custom security attributes

Normalizes those values and writes them into Exchange Online mailbox custom attributes

Optionally builds / updates Dynamic Distribution Lists (DDLs) based on those attributes

Optionally exports DDL memberships to CSV for review

It is designed for environments where HR / source-of-truth data is stored in a custom security attribute set such as PANLCustomUserAttributes and needs to be reflected consistently in EXO.

PANL can be reaplced with any other Custom User Attribute set

####################

# Features

Graph + PANL merge logic
-Reads PANLCustomUserAttributes from customSecurityAttributes
-Fallback to native Entra fields when PANL data is missing

Flexible scoping
-Pull and process users by:
  -Company only
  -Department only
  -Both Company and Department

EXO write-back (with Dry Run)
-Maps merged values to:
  -CustomAttribute1 → Employee type (Staff/Contractor/etc.)
  -CustomAttribute2 → Role
  -CustomAttribute3 → Status (Active/Terminated)

Supports:
  -Dry Run (create CSV only, no EXO changes)
  -Full write to EXO
  -DDL creation & updating
    -Builds RecipientFilter from:
      -Company / Department
      -CustomAttribute1 (employee type)
      -CustomAttribute3 (Active only)
    -Creates or updates three optional DDLs:
      -Staff PLUS Contractors (all staff + contractors)
      -Employees ONLY
      -Contractors ONLY
    -Optionally exports membership of each DDL to CSV

Logging & export
-All processed users exported to C:\ScriptReturns\PANL-*.csv
-Fallback / ambiguous mailbox handling logged to PANL-Fallbacks.log
-DDL membership exports written as <DDLName>-members.csv
