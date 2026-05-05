`High Priority`

1. Personal Data (GDPR)

- Tables employee and user store personal data:
firstname, surname, dob, nationalinsurancenumber, phone, emergencycontactname, emergencycontactphone, salaryaccountnumber, salarysortcode, managerfirstname, managersurname, gender, cardnumber, cardexpirydate, cardcsv.
- Table supplier stores contactname, contactnumber, contactemail.
- Severity: High
- Reason: These are personally identifiable information (PII) and financial data, subject to GDPR and require strong protection (encryption, access controls, audit logging).

2. Sensitive Payment Data

- Table user stores cardnumber, cardexpirydate, cardcsv in plaintext.
- Table employee stores salaryaccountnumber, salarysortcode in plaintext.
- Severity: High
- Reason: Storing payment card data and bank details in plaintext is a major security risk and likely violates PCI DSS and GDPR.

`Medium Priority`

3. Lack of Data Encryption

- No indication of encryption for sensitive fields (PII, payment, or salary data).
- Severity: Medium
- Reason: Encryption at rest and in transit is required for sensitive data.

4. No Audit or Access Control Tables

- No tables for tracking access, changes, or user roles/permissions.
- Severity: Medium
- Reason: GDPR and security best practices require audit trails and access controls.

5. Emergency Contact Data

- emergencycontactname and emergencycontactphone are PII for third parties who may not have consented to data storage.
- Severity: Medium
- Reason: Storing third-party PII without consent is a GDPR risk.

`Low Priority`

6. Data Redundancy

- managerfirstname and managersurname are stored in employee despite a managerid foreign key.
- Severity: Low
- Reason: This can lead to data inconsistency and redundancy.

7. Lack of Normalisation

- Some fields (e.g., gender as free text) could be normalised to reference tables.
- Severity: Low
- Reason: Improves data integrity and reduces errors.

8. No Indexes on Foreign Keys

- No explicit indexes on foreign key columns (e.g., payband, managerid).
- Severity: Low
- Reason: May impact query performance.

`Recommendations`

- High: Immediately encrypt or remove sensitive payment and personal data, implement access controls, and review GDPR compliance.
- Medium: Add audit logging, enforce encryption, and review third-party data storage.
- Low: Refactor schema for normalisation, remove redundant fields, and add indexes for performance.




Here’s a prioritised action plan to address the database issues:

`High Priority`

1. Personal Data (GDPR)

- Technical: Encrypt all PII fields (names, DoB, NI numbers, contact info, account numbers, etc.) at rest and in transit.
- Process: Restrict access to PII to only those who need it for their job.
- Business: Update privacy policies and ensure all staff are trained on GDPR compliance.
- Customer: Notify customers/employees about how their data is used and their rights.

2. Sensitive Payment Data

- Technical: Immediately remove or encrypt card numbers, expiry dates, CSVs, and bank details. Never store card CSVs.
- Process: Use a PCI DSS-compliant third-party payment processor; do not store payment data unless absolutely necessary.
- Business: Review and update contracts with payment processors.
- Customer: Inform users if any data was stored insecurely and what steps are being taken.

`Medium Priority`

3. Lack of Data Encryption

- Technical: Implement database-level encryption for all sensitive fields.
- Process: Regularly audit encryption and access controls.
- Business: Document encryption policies and procedures.

4. No Audit or Access Control Tables

- Technical: Add audit logging for all access and changes to sensitive data.
- Process: Implement role-based access control (RBAC).
- Business: Regularly review access logs and permissions.

5. Emergency Contact Data

- Technical: Encrypt emergency contact fields.
- Process: Obtain explicit consent from emergency contacts before storing their data.
- Business: Update onboarding and HR processes to include consent collection.

`Low Priority`

6. Data Redundancy

- Technical: Remove managerfirstname and managersurname from employee table; use only managerid for lookups.
- Process: Update any business processes or reports that rely on the redundant fields.

7. Lack of Normalisation

- Technical: Create reference tables for fields like gender.
- Process: Update data entry and validation processes.

8. No Indexes on Foreign Keys

- Technical: Add indexes to all foreign key columns to improve performance.

`Summary Table`

| Priority | Issue                        | Solution Summary                                                                 |
|----------|------------------------------|----------------------------------------------------------------------------------|
| High     | PII & Payment Data           | Encrypt all PII, restrict access, update privacy policies, notify users           |
| High     | Payment Data Storage         | Remove/integrate with PCI DSS provider, never store CSV, notify users             |
| Medium   | Lack of Data Encryption      | Implement encryption at rest and in transit, audit, document policies             |
| Medium   | Audit/Access Controls        | Add audit logs, implement RBAC, regularly review permissions and logs             |
| Medium   | Emergency Contact Consent    | Encrypt, collect explicit consent, update HR processes                            |
| Low      | Data Redundancy              | Remove redundant fields (e.g., manager names), update business processes          |
| Low      | Lack of Normalisation        | Add reference tables (e.g., for gender), update validation and data entry         |
| Low      | No Indexes on Foreign Keys   | Add indexes to all FK columns for performance                                     |
