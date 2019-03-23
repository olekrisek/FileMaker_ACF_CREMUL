# FileMaker_ACF_CREMUL
A CREMUL import module for FileMaker using the ACF Plugin

The CREMUL file format parser written for import of payments into a table structure in a FileMaker database. 

The Import Module is written in the ACF language ( Advanced Custom Functions for FileMaker ). To run the code you need to install the ACF Plugin from Webapptech AS. 

The FileMaker database needs three tables:  ( Some of the fields are named in Norwegian, if translated to another language, the ACF function need to be updated as well )

## Table Occurence references

**Preference table**

The function references a preference field at table occurrence: *InvoiceHeader_Preferences::KidExtractInvocieNumberRegex* - this field is referenced two places in the source. This is a regular expression to extract the invoice number from the KID. Typical value: 
```
^0(\d{5})\d$
``` 
This means a zero + 5 digit invoice number and a control digit. The name must be adjusted to fit your preference table occurrence in your application. 

**Invocie Table**

Also - near the end of the package, the variable sql4 is set to the SQL that updates the Invoice table. Updates indexed fields from the calculations for the invoices involved in the import. (Faster query and sort )

This must be updated to fit your needs. 


## Table structure

- PaymentMessage

| FieldName | Type | Description |
| :-- | :-- | :-- |
| MessageRef | Text | The unique File Identifier (pk) |
| MessageDate | Date | The date of the CREMUL message |
| FileName | Text | The name of the imported File |
| ImportedTimeStamp | Timestamp | Date and Time imported |
| ImportedBy | Text | Account name for the user imported the file |
| FilePath | Text | The full file path of the imported file |

- PaymentSequences

| FieldName | Type | Description |
| :-- | :-- | :-- |
| MessageRef | Text | The unique File Identifier ( fk to PaymentMessage ) |
| LIN | Number | The Sequence number for the LIN segment |
| NETSProcessingDate | Date | The date of the payment processing |
| BusCategory | Text | Business Category |
| SequenceAmounth | Number | The total amount for this sequence |
| SequenceBankreference | Text | Bank reference of the Sequence |
| ValDate | Date | Valutation Date |
| ReceiversAccountNumber | Text | The receivers Bank Account Number (Where the money is received) |

- PaymentTransaction

| FieldName | Type | Description |
| :-- | :-- | :-- |
| MessageRef | Text | The unique File Identifier ( fk to PaymentMessage ) |
| LIN_ID | Number | Belong to sequence number ( fk to PaymentSequences) |
| ArkivRef | Text | Transaction Archive Reference |
| OppdragRef | Text | Order Reference |
| TransfAmounth | Number | The amount transferred to the account |
| PayerAccountNumber | Text | The bank account of the payer |
| PayerName | Text | Name of the payer |
| Freetext | Text | Free text |
| SequneceNo | Number | Sequence no inside the LIN segment (PaymentSequences)  |
| ReceivedDate | Date | Payment received date |
| INP | Text |  |
| Proc | Number |  |
| DocRef | Text | Document Reference | 
| ProcAmounth | Number | |
| GeneralIndicator | Number | | 
| KID | Text | The Customer Identification Number | 
| Faktura_nr | Text | The Invoice Number |
| Betalingsmaate | Text | Payment mode, receives the text "CREMUL"  |
| Betaling_signatur | Text | Initials of the user importing the CREMUL file |

A sample database with the tables in it will be published on https://webapptech.no

The ACF function must be compiled with the plugin. For more info, see the ACF manual on: 

https://webapptech.no/manuals/ACF/index.html

