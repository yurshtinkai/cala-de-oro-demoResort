# Requirements Document

## Introduction

The Role Restructure and Review Workflow feature transforms the Cala de Oro Internal Management System's role structure to implement a data entry and review workflow. This feature renames the ADMIN role to MANAGER, redefines IT STAFF as data entry personnel, and establishes a supervisory review process where IT Staff inputs operational data and Managers review, edit, or delete submitted records. The CEO role remains unchanged with executive oversight capabilities.

## Glossary

- **System**: The Cala de Oro Internal Management System
- **IT_Staff**: Staff members responsible for data entry of transactions, bookings, check-ins, and sales records
- **Manager**: Staff members (formerly ADMIN) responsible for reviewing, editing, and deleting operational records
- **CEO**: Executive staff members with read-only access to reports and analytics
- **Data_Entry_Record**: Any operational record created by IT Staff including transactions, bookings, check-ins, check-outs, and sales
- **Submitted_Record**: A Data Entry Record that has been saved by IT Staff and is no longer editable by the creator
- **Audit_Trail**: A chronological record of all create, edit, and delete operations on Data Entry Records
- **Operational_Data**: Transactions, bookings, check-ins, check-outs, room status changes, and sales records
- **User_Account**: A system account with credentials, role assignment, and access permissions

## Requirements

### Requirement 1: Role Name Migration

**User Story:** As a system administrator, I want the ADMIN role renamed to MANAGER throughout the system, so that the role name accurately reflects supervisory responsibilities.

#### Acceptance Criteria

1. THE System SHALL rename all instances of "ADMIN" to "MANAGER" in the user interface
2. THE System SHALL update all "ADMIN" role identifiers to "MANAGER" in the data storage layer
3. THE System SHALL preserve all existing permission assignments during the role name migration
4. THE System SHALL update all existing user accounts with ADMIN role to have MANAGER role
5. THE System SHALL maintain backward compatibility for session data during the migration period

### Requirement 2: IT Staff Data Entry Permissions

**User Story:** As an IT Staff member, I want to create operational data records, so that I can input daily transactions and bookings into the system.

#### Acceptance Criteria

1. WHEN IT_Staff is authenticated, THE System SHALL grant permission to create new Data_Entry_Records
2. THE System SHALL allow IT_Staff to create transaction records with all transaction types
3. THE System SHALL allow IT_Staff to create booking records with guest and room information
4. THE System SHALL allow IT_Staff to perform check-in operations and record guest arrival
5. THE System SHALL allow IT_Staff to perform check-out operations and record guest departure
6. THE System SHALL allow IT_Staff to create sales records for resort outlets and equipment rentals
7. WHEN IT_Staff creates a Data_Entry_Record, THE System SHALL automatically tag the record with the creator's user identifier
8. WHEN IT_Staff creates a Data_Entry_Record, THE System SHALL automatically set the creation timestamp

### Requirement 3: IT Staff Edit Restrictions

**User Story:** As a system administrator, I want IT Staff to be unable to edit records after submission, so that data integrity is maintained and supervisory review is enforced.

#### Acceptance Criteria

1. WHEN IT_Staff attempts to edit a Submitted_Record, THE System SHALL deny the edit operation
2. WHEN IT_Staff attempts to delete a Submitted_Record, THE System SHALL deny the delete operation
3. THE System SHALL display a read-only view of Submitted_Records to IT_Staff
4. WHEN IT_Staff views their submission history, THE System SHALL indicate which records are no longer editable
5. IF IT_Staff needs to correct a Submitted_Record, THEN THE System SHALL provide a mechanism to request Manager assistance

### Requirement 4: Manager Review Access

**User Story:** As a Manager, I want to view all records submitted by IT Staff, so that I can review operational data for accuracy and completeness.

#### Acceptance Criteria

1. WHEN Manager is authenticated, THE System SHALL display all Data_Entry_Records created by IT_Staff
2. THE System SHALL allow Manager to filter Data_Entry_Records by date range
3. THE System SHALL allow Manager to filter Data_Entry_Records by record type
4. THE System SHALL allow Manager to filter Data_Entry_Records by creator
5. THE System SHALL display the creator's name and creation timestamp for each Data_Entry_Record
6. THE System SHALL sort Data_Entry_Records with most recent submissions first by default

### Requirement 5: Manager Edit and Delete Permissions

**User Story:** As a Manager, I want full edit and delete permissions on all operational records, so that I can correct errors and maintain data quality.

#### Acceptance Criteria

1. THE System SHALL grant Manager permission to edit any Data_Entry_Record
2. THE System SHALL grant Manager permission to delete any Data_Entry_Record
3. WHEN Manager edits a Data_Entry_Record, THE System SHALL preserve the original creator information
4. WHEN Manager edits a Data_Entry_Record, THE System SHALL record the Manager's user identifier as the editor
5. WHEN Manager edits a Data_Entry_Record, THE System SHALL record the edit timestamp
6. WHEN Manager deletes a Data_Entry_Record, THE System SHALL record the deletion in the Audit_Trail
7. THE System SHALL allow Manager to edit all fields in a Data_Entry_Record except the original creator and creation timestamp

### Requirement 6: Audit Trail for Record Modifications

**User Story:** As a Manager, I want to see the complete history of changes to operational records, so that I can track who made what changes and when.

#### Acceptance Criteria

1. WHEN a Data_Entry_Record is created, THE System SHALL record the creator, creation timestamp, and initial values in the Audit_Trail
2. WHEN a Data_Entry_Record is edited, THE System SHALL record the editor, edit timestamp, and modified fields in the Audit_Trail
3. WHEN a Data_Entry_Record is deleted, THE System SHALL record the deleter, deletion timestamp, and deleted record snapshot in the Audit_Trail
4. THE System SHALL allow Manager to view the Audit_Trail for any Data_Entry_Record
5. THE System SHALL display Audit_Trail entries in chronological order with oldest first
6. THE System SHALL preserve Audit_Trail entries for deleted records
7. THE System SHALL display field-level changes in Audit_Trail entries showing before and after values

### Requirement 7: IT Staff Submission History

**User Story:** As an IT Staff member, I want to view a history of my submitted records, so that I can track my work and verify that records were entered correctly.

#### Acceptance Criteria

1. THE System SHALL provide IT_Staff with a view of all Data_Entry_Records they created
2. THE System SHALL allow IT_Staff to filter their submission history by date range
3. THE System SHALL allow IT_Staff to filter their submission history by record type
4. WHEN displaying submission history to IT_Staff, THE System SHALL indicate whether each record has been edited by Manager
5. WHEN a record has been edited by Manager, THE System SHALL display the most recent edit timestamp
6. THE System SHALL allow IT_Staff to view the current state of their submitted records in read-only mode

### Requirement 8: Manager Notification for New Submissions

**User Story:** As a Manager, I want to be notified when IT Staff submits new records, so that I can review them in a timely manner.

#### Acceptance Criteria

1. WHEN IT_Staff creates a Data_Entry_Record, THE System SHALL display a notification indicator in the Manager dashboard
2. THE System SHALL maintain a count of unreviewed Data_Entry_Records for Manager
3. WHEN Manager views a Data_Entry_Record for the first time, THE System SHALL mark it as reviewed
4. THE System SHALL decrease the unreviewed count when Manager marks a record as reviewed
5. THE System SHALL allow Manager to mark multiple records as reviewed in batch operations

### Requirement 9: User Account Management Permissions

**User Story:** As a system administrator, I want to ensure that only Managers can create and manage user accounts, so that proper access control is maintained.

#### Acceptance Criteria

1. THE System SHALL grant Manager permission to create new User_Accounts
2. THE System SHALL grant Manager permission to edit existing User_Accounts
3. THE System SHALL grant Manager permission to deactivate User_Accounts
4. THE System SHALL allow Manager to assign IT_Staff role to User_Accounts
5. THE System SHALL allow Manager to assign Manager role to User_Accounts
6. THE System SHALL prevent Manager from creating or modifying CEO role User_Accounts
7. THE System SHALL deny IT_Staff all user management operations
8. WHEN Manager creates a User_Account, THE System SHALL require email, password, full name, and role assignment

### Requirement 10: CEO Role Preservation

**User Story:** As a CEO, I want my existing read-only access to reports and analytics maintained, so that I can continue to perform executive oversight.

#### Acceptance Criteria

1. THE System SHALL preserve CEO access to all financial reports
2. THE System SHALL preserve CEO access to analytics dashboards
3. THE System SHALL maintain CEO read-only restrictions on Operational_Data
4. THE System SHALL prevent CEO from creating, editing, or deleting Data_Entry_Records
5. THE System SHALL prevent CEO from accessing user management functions
6. THE System SHALL allow CEO to view Audit_Trail entries for transparency

### Requirement 11: Role-Based Dashboard Access

**User Story:** As a user, I want to see a dashboard appropriate for my role, so that I can efficiently access the features I need.

#### Acceptance Criteria

1. WHEN IT_Staff logs in, THE System SHALL display a data entry focused dashboard
2. WHEN Manager logs in, THE System SHALL display a supervisory review dashboard with pending records
3. WHEN CEO logs in, THE System SHALL display the executive analytics dashboard
4. THE System SHALL include quick action buttons on each dashboard relevant to the user's role
5. THE System SHALL display role-appropriate statistics and metrics on each dashboard

### Requirement 12: Permission Enforcement Across All Modules

**User Story:** As a system administrator, I want role permissions enforced consistently across all system modules, so that security policies are reliably applied.

#### Acceptance Criteria

1. THE System SHALL enforce IT_Staff data entry permissions in the transaction module
2. THE System SHALL enforce IT_Staff data entry permissions in the booking module
3. THE System SHALL enforce IT_Staff data entry permissions in the room management module
4. THE System SHALL enforce Manager edit and delete permissions in all operational modules
5. THE System SHALL enforce CEO read-only permissions in all modules
6. WHEN a user attempts an unauthorized operation, THE System SHALL display an error message indicating insufficient permissions
7. WHEN a user attempts an unauthorized operation, THE System SHALL log the attempt in the security audit log

### Requirement 13: Existing Session Migration

**User Story:** As a system administrator, I want existing user sessions to be updated with new role permissions, so that users do not need to log out and log back in.

#### Acceptance Criteria

1. WHEN the role restructure is deployed, THE System SHALL update all active ADMIN sessions to Manager sessions
2. THE System SHALL update session permissions for all active IT_Staff sessions to reflect new data entry restrictions
3. THE System SHALL preserve CEO sessions without modification
4. THE System SHALL allow users to continue working without re-authentication after the role restructure
5. IF a session cannot be migrated automatically, THEN THE System SHALL prompt the user to log out and log back in

### Requirement 14: Data Entry Record Metadata

**User Story:** As a Manager, I want to see detailed metadata for every record, so that I can understand the context and history of each entry.

#### Acceptance Criteria

1. THE System SHALL display the creator's full name on every Data_Entry_Record
2. THE System SHALL display the creation timestamp in a human-readable format on every Data_Entry_Record
3. WHEN a Data_Entry_Record has been edited, THE System SHALL display the most recent editor's full name
4. WHEN a Data_Entry_Record has been edited, THE System SHALL display the most recent edit timestamp
5. THE System SHALL display the total number of edits on each Data_Entry_Record
6. THE System SHALL provide a link to view the complete Audit_Trail from the record detail view

### Requirement 15: Reporting and Analytics Integration

**User Story:** As a CEO, I want reports to include information about who created and modified records, so that I can understand operational workflows.

#### Acceptance Criteria

1. THE System SHALL include creator information in transaction reports
2. THE System SHALL include editor information in transaction reports when records have been modified
3. THE System SHALL allow filtering reports by IT_Staff member to analyze individual performance
4. THE System SHALL allow filtering reports by Manager to analyze supervisory activity
5. THE System SHALL provide a summary report showing total records created by each IT_Staff member per time period
6. THE System SHALL provide a summary report showing total records edited by each Manager per time period
