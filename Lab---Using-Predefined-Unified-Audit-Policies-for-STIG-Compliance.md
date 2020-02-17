# Lab - Using Predefined Unified Audit Policies for STIG Compliance



- <a href='#Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-BeforeYouBegin'>Before You Begin</a>

- <a href='#Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP1:DisplaythePredefinedUnifiedAuditPoliciesImplemented'>STEP 1: Display the Predefined Unified Audit Policies Implemented</a>

- <a href='#Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP2:UnderstandtheActionsAuditedbythePredefinedUnifiedAuditPolicies'>STEP 2: Understand the Actions Audited by the Predefined Unified Audit Policies</a>

- <a href='#Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP3:EnablethePredefinedUnifiedAuditPolicies'>STEP 3: Enable the Predefined Unified Audit Policies</a>

- <a href='#Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-NextLabs'>Next Labs</a>





<h2 id="Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-BeforeYouBegin">Before You Begin


### Objectives

This practice shows how to use predefined unified audit policies to implement Security Technical Implementation Guides (STIG) audit requirements.


### Requirements

To complete this lab, you need to have the following:

- In Oracle Database 19c CDB (CDB19) a PDB (PDB19) and in Oracle Database 20c CDB (CDB20) a PDB (PDB20)
- A listener running to access to CDBs and PDBs
- SQL*Plus to connect the CDBs and PDBs
<h2 id="Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP1:DisplaythePredefinedUnifiedAuditPoliciesImplemented">**STEP 1**: Display the Predefined Unified Audit Policies Implemented

- Connect to ``PDB19`` as ``SYSTEM`` and verify which predefined unified audit policies are implemented.


  ``$ sqlplus system@PDB19 ``

  ``Enter password: <em>password </em>``

  ``Connected. ``

  ``SQL> SELECT DISTINCT policy_name FROM audit_unified_policies ORDER BY 1; ``

  

  ``POLICY_NAME``

  `` `` ``--------------------------------------------------------------------------------``

  `` `` ``DBA_MGT_USERS``

  `` `` ``ORA_ACCOUNT_MGMT``

  `` `` ``ORA_CIS_RECOMMENDATIONS``

  `` `` ``ORA_DATABASE_PARAMETER``

  `` `` ``ORA_DV_AUDPOL``

  `` `` ``ORA_DV_AUDPOL2``

  `` `` ``ORA_LOGON_FAILURES``

  `` `` ``ORA_RAS_POLICY_MGMT``

  `` `` ``ORA_RAS_SESSION_MGMT``

  `` `` ``ORA_SECURECONFIG ``

  

  ``SQL>``



- Connect to ``PDB20`` as ``SYSTEM`` and verify which predefined unified audit policies are implemented.


  ``$ sqlplus system@PDB20``

  `` Enter password: <em>password</em>``

  `` Connected.``

  `` SQL> SELECT DISTINCT policy_name FROM audit_unified_policies ORDER BY 1;``



  ``POLICY_NAME``

  ``--------------------------------------------------------------------------------``

  ``ORA_ACCOUNT_MGMT``

  **``ORA_ALL_TOPLEVEL_ACTIONS``**

  ``ORA_CIS_RECOMMENDATIONS``

  ``ORA_DATABASE_PARAMETER``

  ``ORA_DV_AUDPOL``

  ``ORA_DV_AUDPOL2``

  ``ORA_LOGON_FAILURES``

  **``ORA_LOGON_LOGOFF``**

  ``ORA_RAS_POLICY_MGMT``

  ``ORA_RAS_SESSION_MGMT``

  ``ORA_SECURECONFIG``

  **``ORA_STIG_RECOMMENDATIONS ``**



  ``12 rows selected. ``

  

  ``SQL>``<h2 id="Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP2:UnderstandtheActionsAuditedbythePredefinedUnifiedAuditPolicies">**STEP 2**: Understand the Actions Audited by the Predefined Unified Audit Policies

- Observe the three new predefined unified audit policies implemented. Are these policies enabled to satisfy STIG compliance?


  ``SQL> SELECT * FROM audit_unified_enabled_policies WHERE policy_name IN ('ORA_ALL_TOPLEVEL_ACTIONS','ORA_LOGON_LOGOFF','ORA_STIG_RECOMMENDATIONS');``

  

  ``no rows selected``

  

  ``SQL>``



  None of these are enabled.

  

- Before enabling any of these policies, understand which actions they would audit. Verify the actions audited by``ORA_STIG_RECOMMENDATIONS``.


  ``SQL> COL audit_option FORMAT A26``

  ``SQL> COL AUDIT_OPTION_TYPE FORMAT A16``

  ``SQL> COL OBJECT_SCHEMA FORMAT A4``

  ``SQL> COL OBJECT_NAME FORMAT A22``

  ``SQL> COL OBJECT_TYPE FORMAT A7``

  ``SQL> SELECT audit_option, audit_option_type, object_schema, object_name, object_type``

  ``     FROM  audit_unified_policies``

  ``     WHERE policy_name = 'ORA_STIG_RECOMMENDATIONS';``



  ``AUDIT_OPTION               AUDIT_OPTION_TYP OBJE OBJECT_NAME            OBJECT_``

  ``-------------------------- ---------------- ---- ---------------------- -------``

  ``ALTER SESSION              SYSTEM PRIVILEGE NONE NONE                   NONE``

  ``CREATE TABLE               STANDARD ACTION  NONE NONE                   NONE``

  ``DROP TABLE                 STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER TABLE                STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE SYNONYM             STANDARD ACTION  NONE NONE                   NONE``

  ``DROP SYNONYM               STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE VIEW                STANDARD ACTION  NONE NONE                   NONE``

  ``DROP VIEW                  STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE PROCEDURE           STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER PROCEDURE            STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER DATABASE             STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER USER                 STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER SYSTEM               STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE USER                STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE ROLE                STANDARD ACTION  NONE NONE                   NONE``

  ``DROP USER                  STANDARD ACTION  NONE NONE                   NONE``

  ``DROP ROLE                  STANDARD ACTION  NONE NONE                   NONE``

  ``SET ROLE                   STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE TRIGGER             STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER TRIGGER              STANDARD ACTION  NONE NONE                   NONE``

  ``DROP TRIGGER               STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE PROFILE             STANDARD ACTION  NONE NONE                   NONE``

  ``DROP PROFILE               STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER PROFILE              STANDARD ACTION  NONE NONE                   NONE``

  ``DROP PROCEDURE             STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE MATERIALIZED VIEW   STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER MATERIALIZED VIEW    STANDARD ACTION  NONE NONE                   NONE``

  ``DROP MATERIALIZED VIEW     STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE TYPE                STANDARD ACTION  NONE NONE                   NONE``

  ``DROP TYPE                  STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER ROLE                 STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER TYPE                 STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE TYPE BODY           STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER TYPE BODY            STANDARD ACTION  NONE NONE                   NONE``

  ``DROP TYPE BODY             STANDARD ACTION  NONE NONE                   NONE``

  ``DROP LIBRARY               STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER VIEW                 STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE FUNCTION            STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER FUNCTION             STANDARD ACTION  NONE NONE                   NONE``

  ``DROP FUNCTION              STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE PACKAGE             STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER PACKAGE              STANDARD ACTION  NONE NONE                   NONE``

  ``DROP PACKAGE               STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE PACKAGE BODY        STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER PACKAGE BODY         STANDARD ACTION  NONE NONE                   NONE``

  ``DROP PACKAGE BODY          STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE LIBRARY             STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE JAVA                STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER JAVA                 STANDARD ACTION  NONE NONE                   NONE``

  ``DROP JAVA                  STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE OPERATOR            STANDARD ACTION  NONE NONE                   NONE``

  ``DROP OPERATOR              STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER OPERATOR             STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE SPFILE              STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER SYNONYM              STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER LIBRARY              STANDARD ACTION  NONE NONE                   NONE``

  ``DROP ASSEMBLY              STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE ASSEMBLY            STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER ASSEMBLY             STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER PLUGGABLE DATABASE   STANDARD ACTION  NONE NONE                   NONE``

  ``CREATE LOCKDOWN PROFILE    STANDARD ACTION  NONE NONE                   NONE``

  ``DROP LOCKDOWN PROFILE      STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER LOCKDOWN PROFILE     STANDARD ACTION  NONE NONE                   NONE``

  ``ADMINISTER KEY MANAGEMENT  STANDARD ACTION  NONE NONE                   NONE``

  ``ALTER DATABASE DICTIONARY  STANDARD ACTION  NONE NONE                   NONE``

  ``GRANT                      STANDARD ACTION  NONE NONE                   NONE``

  ``REVOKE                     STANDARD ACTION  NONE NONE                   NONE``

  ``ALL                        OLS ACTION       NONE NONE                   NONE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_SCHEDULER         PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_JOB               PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_RLS               PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_REDACT            PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_TSDP_MANAGE       PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_TSDP_PROTECT      PACKAGE``

  ``EXECUTE                    OBJECT ACTION    SYS  DBMS_NETWORK_ACL_ADMIN PACKAGE``



  ``75 rows selected.``



  ``SQL>``

  The policy once enabled audits all major actions that could damage the security and the smooth running of the database, and also all Oracle Label Security actions. This result shows that you should enable the policy for all users.

  

- Verify the actions audited by ``ORA_ALL_TOPLEVEL_ACTIONS``.


  ``SQL> COL audit_option FORMAT A6``

  ``SQL> COL object_name FORMAT A11``

  ``SQL> COL audit_only_toplevel FORMAT A22``

  ``SQL> SELECT audit_option, audit_option_type, object_schema, object_name, object_type, audit_only_toplevel``

  ``     FROM  audit_unified_policies``

  ``     WHERE policy_name = 'ORA_ALL_TOPLEVEL_ACTIONS';``



  ``AUDIT_ AUDIT_OPTION_TYP OBJE OBJECT_NAME OBJECT_ AUDIT_ONLY_TOPLEVEL``

  ``------ ---------------- ---- ----------- ------- ----------------------``

  ``ALL    STANDARD ACTION  NONE NONE        NONE    YES``



  ``SQL>``

  The policy once enabled audits all top level actions of privileged users on any object that could damage the security of the database. This result shows that you should enable the policy for all users.



- Verify the actions audited by``ORA_LOGON_LOGOFF``.


  ``SQL> COL audit_option FORMAT A6``

  ``SQL> COL object_name FORMAT A11``

  ``SQL> COL audit_only_toplevel FORMAT A22``

  ``SQL> SELECT audit_option, audit_option_type, object_schema, object_name, object_type, audit_only_toplevel``

  ``     FROM  audit_unified_policies``

  ``     WHERE policy_name = 'ORA_LOGON_LOGOFF';``



  ``AUDIT_ AUDIT_OPTION_TYP OBJE OBJECT_NAME OBJECT_ AUDIT_ONLY_TOPLEVEL``

  ``------ ---------------- ---- ----------- ------- ----------------------``

  ``LOGON  STANDARD ACTION  NONE NONE        NONE    NO``

  ``LOGOFF STANDARD ACTION  NONE NONE        NONE    NO``



  ``SQL>``

  The policy once enabled audits all connection and disconnections that could display unsecure connections to the database. This policy is required for both the Center for Internet Security (CIS) and Security for Technical Implementation Guides (STIG) requirements.<h2 id="Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-STEP3:EnablethePredefinedUnifiedAuditPolicies">**STEP 3**: Enable the Predefined Unified Audit Policies

- Enable all three audit policies for all users.


  ``SQL> AUDIT POLICY ORA_STIG_RECOMMENDATIONS;``



  ``Audit succeeded.``



  ``SQL> AUDIT POLICY ORA_ALL_TOPLEVEL_ACTIONS;``



  ``Audit succeeded.``



  ``SQL> AUDIT POLICY ORA_LOGON_LOGOFF;``



  ``Audit succeeded.``



  ``SQL> EXIT``

  ``$``<h2 id="Lab-UsingPredefinedUnifiedAuditPoliciesforSTIGCompliance-NextLabs">Next Labs

- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-auditing-actions-connected-sessions.html" class="external-link" rel="nofollow">Auditing Actions on Connected Sessions </a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-enforcing-unified-audit-policies-current-user.html" class="external-link" rel="nofollow">Enforcing Unified Audit Policies on the Current User </a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-forcing-upgraded-password-file-be-case-sensitive.html" class="external-link" rel="nofollow">Forcing Upgraded Password File to be Case Sensitive</a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-configuring-syslog-destination-common-unified-audit-policies.html" class="external-link" rel="nofollow">SYSLOG Destination for Common Unified Audit Policies </a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-managing-oracle-blockchain-tables-and-rows.html" class="external-link" rel="nofollow">Managing Blockchain Tables and Rows</a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-setting-default-tablespace-encryption-algorithm.html" class="external-link" rel="nofollow">Setting the Default Tablespace Encryption Algorithm</a> 
- Lab -  <a href="https://docs-uat.us.oracle.com/en/database/oracle/oracle-database/20/ftnew/practice-preventing-local-users-blocking-common-operations.html" class="external-link" rel="nofollow">Preventing Local Users from Blocking Common Operations</a>   


 



