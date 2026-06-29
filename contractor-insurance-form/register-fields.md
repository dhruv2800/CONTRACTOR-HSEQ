# Register field map

Extracted from `Recycling_Parks_Contractor_Management_Register.xlsx`
(sheet *Contractor Management*). This is the source of truth the Form and List must match.

## Contractor Details
| Field | Type | Notes |
|-------|------|-------|
| Contractor Name | Text | Company / business name |
| Services Provided | Text (long) | e.g. "Mechanical services and welding" |
| Transport Operator | Choice: Yes / No | Gates the CoR pre-qual |
| Contact | Text | Contact person's name |
| Email | Email | Contact email |
| PHB | Text | (column in register — confirm meaning, likely "Prime Hauling/Booking" ref or phone) |

## Pre-Qualifications
| Field | Type | Notes |
|-------|------|-------|
| HSE Pre-Qualification Completed | Choice: Yes / No | **All contractors** |
| CoR Pre-Qualification Completed | Choice: Yes / No / N/A | **Transport operators only** (Chain of Responsibility) |
| Today's Date | Date | Date of submission/assessment |

## Insurances (Certificate of Currency — COC)
| Field | Type | Notes |
|-------|------|-------|
| Commercial Plant & MV — COC Expiry Date | Date | + certificate file |
| Public Liability — COC Expiry Date | Date | + certificate file |
| Workers Compensation — COC Expiry Date | Date | + certificate file |

## Status (internal — you fill these, not the contractor)
| Field | Type | Notes |
|-------|------|-------|
| Status | Choice: Compliant / Non-Compliant | Auto-set by Flow #2 when a cert lapses |
| Docs uploaded to Wanless Operations | Choice: Yes / No | Internal tracking |
| Approved by | Text | Approver name |

> Dropdown source values found in the workbook's *Data* sheet:
> Yes/No, Yes/No, and Compliant / Non-Compliant (plus N/A).
