# Changelog

## [0.7.2] - 2026-05-11

### Added

- Ability to delete a merchant's terminal file in cashback when editing
- Ability to reprocess transactions through the "Merchants" section: "Recalculate cashback" button in the terminal editing block is activated when adding new terminals
- Logic for automatic recalculation of transactions for the current month when adding new terminals to a merchant

![Demonstration of cashback recalculation by merchant](images/changelog/all_types_recount.gif)

### Changed

- Updated Node.js version
- Changed the merchant form: split into two tabs — "Merchant Settings" and "Terminal Settings"; added search by merchant name and separate icons for editing settings and terminals
- Removed card type filter on all cashback data retrieval routes in the mobile gateway (except co-branding)
- Changed the mapper page for Kafka/webhooks: updated mapper verification logic and results display
- New manual terminal addition field now displays at the top when adding multiple terminals
- Changed the name of the "Add Manually" block to "Add Terminals Manually" in the merchant form
- "Recalculate Cashback" button is now active for new merchants (not tied to active cashback)

### Fixed

- After session timeout expired, the Unauthorized Access page was displayed instead of the login page
- Operation type was not displayed in operation details in user analytics
- Unable to delete a merchant's terminal file in cashback when editing merchants
- Null was automatically set in the name and location fields when adding terminals to a merchant from a file
- Pagination was cut off when editing terminals in the "Merchants" section
- Incorrect search by terminal ID through the search line in the merchant listing
- Bottom highlight of the active page was cut off when viewing merchant terminals
- Error when adding a previously exported merchant terminal file
- Selected terminal field for archival was cut off
- Tooltip size did not match the text length in it in cashback settings
- Page title and description of the first currency rate setup page were not translated; "Currency for conversion" and "API for getting rates" fields were displayed with different heights
- Non-clickable text "Add new link" was displayed on the form for adding promotional referral link

## [0.7.1] - 2026-04-29

### Fixed

- Save button for general settings of default cashback did not work: after editing settings and clicking "Save", the save request was not executed
- Unable to add Deeployalty merchant to default cashback: the system returned an error updating Deeployalty merchants

## [0.6.1] - 2026-04-27

### Added

- Fixing in a separate fact table for receiving available cashbacks by user (view without selection) with webhook configuration
- Client segmentation by merchant in default cashback: ability to set a separate cashback percentage for a segment and manage the availability of default rewards for clients outside segments
- Ability to download an example template file for terminals to configure segmentation
- Export button for segment client list of a merchant
- `transactionId` field in transaction import
- E2e test launch configuration within CI
- Test mode: ability to add cashbacks for different (including past) periods through ENV parameter

### Changed

- Segment management logic moved exclusively to default cashback (removed duplication in personal)
- Transaction import: added support for `cardType` field
- Transition from `TransactionProcessStatus` to `TransactionCashbackDecision`
- Clarified logic for maximum cashback amount per merchant
- Changed "Accrual Terms" page and "Cashback Withdrawal Terms" section
- Unified field formats across all system forms
- Changed the name of archived parameters in all menus
- Clarified webhook settings
- Merchant settings: corrections to display and logic

### Fixed

- Cashback was not accrued for a client from a merchant segment when all default cashback conditions were met
- Double cashback accrual on a group; missing priority of personal cashback over default
- Group cashback percentage was not added to basic accrual when MCC code matched
- Default cashback with filled `Merchant Name` field was accrued without considering merchant name in transaction by segment
- Merchant was not identified by terminal when processing transaction
- Merchant was not identified by terminal with incorrect `merchantName`
- Merchant was not identified by terminal in the "Merchants" section
- System did not identify merchant for valid transaction in analytics
- Default cashback of merchant by terminal was not accrued in cashback
- Backend skipped merchant selection for client not included in default cashback
- Merchant name was not displayed in transaction listing of analytics during reversal
- Group/merchant was not identified when transaction minimum amount was insufficient for accrual
- GET `/api/v2/cashback/positions` did not return cashbacks by segments for client
- POST `/api/co-branded-cashback/customer` returned 400 despite active co-branded cashback
- Co-brand. Web API returned empty array of selected groups and merchants
- Co-brand. Mobile API: 404 when getting client's selected positions for co-branding
- Mobile API returned incorrect % cashback for client in segment
- Mobile API (`POST /api/customer-cashback/{id}`) did not accept array
- Mobile API incorrectly returned withdrawal value in settings route
- Personal cashback: web gateway returned duplicates of selected merchant/group by client in analytics
- Personal cashback was saved without uploaded client file
- Personal cashback accepted invalid client files
- System rejected segment client file with valid Customer ID
- Terminal file was not added to cashback with segment
- Terminal file upload in cashback ended with error when using template
- If merchant has 1 terminal — it could not be deleted
- Adding segmentation added new merchant to default cashback
- Incorrect logic of toggle operation in default cashback segmentation
- Default cashback was not saved if merchant minimum amount was greater than maximum cashback amount
- 500 when deleting merchant in current cashback
- 500 when editing short description to 2000+ characters
- 500 (backend crash) on stage — regression eliminated
- Deeployalty duplicated large number of default cashbacks to next month
- Webhook about receipt NOT-matching was not sent to Deeployalty
- Merchant campaign terminal count from Deeployalty was not duplicated in default cashback
- Error editing merchant when adding invalid terminal values
- Editing operation type name worked incorrectly depending on language localization
- Webhook about receipt NOT-matching was not sent to Deeployalty
- User registration error by referral code
- Incorrect calculation of registration limit by referral link
- "Last update date" field was not updated after adding clients to personal cashback
- Missing confirmation window when deleting clients from merchant segment
- When reaching maximum segment limit, message "Merchant limit reached" was displayed
- Save button for general settings of default cashback did not work
- "Docs" button did not work
- Localization key was displayed in merchant segment form
- Language of messages did not match language of interface locale
- Missing validation of amount fields in "Cashback Withdrawal Terms" subsection; fields allowed negative values
- Incorrect error message when withdrawing cashback (same response for different denial scenarios in `/withdraw` route)
- It was possible to delete Card Product from default cashback of future period without error
- File import without card type did not work
- `Cannot GET` errors when loading cashback page
- Page scrolled to header when switching pagination
- Layout "floats" depending on number of digits in pagination
- Centered tooltip relative to line in transaction import details
- Missing asterisk for required field next to "Group" field in each cashback type
- "Cashback" menu item in sidebar had incorrect text alignment and displayed extra tooltip on hover
- Lexical error in default cashback settings
- Favicon installed on changelog page
- Business admin granted archival rights

## [0.6.0] - 2026-03-25

### Added

- Multi-currency support: currency rate directory (NBU), automatic transaction conversion to system currency, saving conversion data (original amount, currency, rate) along with transaction
- Separate `INVALID_TRANSACTIONS` table for invalid transactions (missing currency, unknown rate, etc.) with reason displayed in user analytics
- New cashback type — co-branded: support for card type settings, parallel operation with default cashback, separate mobile endpoints for retrieving settings and positions
- `Merchant Name` field as additional condition for transaction matching by merchant (along with Terminal ID)
- Additional `terminal_name` and `terminal_location` fields when adding terminals by merchant
- Counter for total number of uploaded terminals for each merchant across all cashback types
- Endpoint for getting cashback data for specific transaction (search by transaction ID)
- Paginated endpoint for getting full cashback history of user (duplicated to mobile gateway)
- `bannerUrl` field in mobile endpoint `/cashbacks/positions` (only for merchant type positions)
- `accountId` field in `CASHBACK_HISTORY` — stores account to which funds were withdrawn
- Return of `cashbackHistoryId` in response of `/withdraw` endpoint (for reversal operations support)
- [Deeployalty] Sending webhook about selected cashback by user to Deeployalty when changing customer settings
- Deployment of cashback module for "Clearing House" client
- Display of Deeployalty marker on merchants when selecting them during cashback creation
- Ability to view errors by rows when importing transactions
- Display of required fields in all system forms

### Changed

- [Deeployalty] Updated terminal synchronization logic: terminals in "Merchants" section are only supplemented (not deleted), in merchant cashback block — actual terminals of current campaign
- Analytics on frontend displays transaction dates in local time (instead of UTC)
- `accountId` field in `/cashback-histories/withdraw` endpoint became required
- Statistics page hidden (data was hardcoded, did not reflect actual values)
- Unified pagination on all tabs with table display

### Fixed

- Unable to change settings of Deeployalty merchant for cashback on next month
- Cashback was not deducted when refund with currency conversion
- 500 error when withdrawing cashback
- 500 error when withdrawing funds to "Clearing House" client
- Authorization error on Oschadbank server (dev)
- Database connection error (dev Oschadbank)
- Co-branded cashback accrual occurred without considering card type
- Active co-branded cashback for current month available for editing
- Unable to disable currency conversion during initial system setup
- "Save" button was active even without changes in currency API settings
- UAH transaction passed only in uppercase (UPPERCASE)
- Oracle: `IN()` limitation to 1000 elements — fixed with batch processing
- After logout, redirect occurred to `/setup-database` instead of `/login`
- Missing redirect to login page after confirming logout in Settings section
- Column names `CreatedAt`, `Parent ID`, `LDAP Filter` were not translated to Ukrainian in respective sections
- Currency names in "Currency Rate" section were displayed without country name in Ukrainian localization
- Amount in transaction list was displayed without conversion to system currency
- "Documentation" button opened main page instead of documentation
- Merchant terminal file was downloaded empty
- Terminal counter in merchant card counted terminals from cashback settings
- Errors when working with WebPlatform API (Clearing House)

## [0.5.0] - 2026-02-26

### Added
- Time zone settings for correct transaction processing
- Added DB_ENCRYPT & DB_TRUST_SERVER_CERTIFICATE settings for MSSQL connection
- Built-in documentation link in admin interface
- Support for MD-format in "Description" field by merchant and value delivery on API
- Kafka Enrichment Microservice: integration with Client API, transaction enrichment, message consumption from Kafka
- MapperCheckService for validating DataSourceMapper before Kafka operations
- Paginated endpoint for cashback history with date filters
- TRANSACTION_FAILURES table and workflow for failed transactions
- GroupCashbackHistoryItemResponse for simplified cashback history in transaction details
- maxAmount at merchant level
- banner URL field for merchants
- Short description for merchants and groups
- processErrorMessage in transaction processing responses

### Changed
- Changed cashback editing logic
- [Deeployalty] Retrieve max cashback amount per merchant field during Deeployalty integration
- [Deeployalty] Cron launch every 1 hour to update cashback offers
- Removed CurrentCashbackResponse and related logic
- Refactored cashback service logic for personal/default cashbacks
- Refactored refund transaction processing

### Fixed
- Cron job overlap
- Cashback retrieval by certain type not working
- Transaction import available to non-admin
- Incorrect prod link for Deeployalty integration for cashbacks
- HTTPS dev stage error
- Rada PROD: merchants marked as from Deeployalty
- Referral program available to non-admin
- Prod rada: missing previously added users in configurator
- Prod rada: GET error when navigating to webhooks section
- Unable to export customer list for personal cashback
- Transaction import — tooltip message not translated when changing interface language
- Errors when working with WebPlatform API clearing house
- Blocked merchant settings fields for Deeployalty merchant when adding to default/personal cashback
- Error adding merchant to current default cashback if it contained Deeployalty merchant
- Incorrect cashback accrued by merchant
- Personal cashback creation
- Deeployalty time zone

## [0.4.0] - 2026-02-12

### Added
- CRUD for cashback operation types in admin panel
- Automatic initialization of nv-keys in Vault on first run
- Terminal list import and export for cashback by merchant
- Transaction upload section in admin panel
- Settings section in admin panel with ability to:
  - update LDAP connection and role mappers
  - manage manually created users
  - change access permissions for editing past periods of cashbacks
  - configure webhooks
  - add new access keys for Mobile Gateway (authorization)
  - change database settings (not recommended after initial setup)
  - add new data sources (e.g., transaction upload from Kafka)
- New endpoint `GET /api/v2/cashback/current` to get current cashback settings, positions, and selected card products in one request

### Changed
- Webhook mapping routes with field relocation in subscriptions
- "Description" field validation for merchants: all special characters allowed
- Removed deprecated endpoints:
  - `GET /customer-cashback/chosen/:id`
  - `GET /settings`
  - `GET /allowed-cashbacks/positions`
  - `GET /receipt/transaction/:id`
- Personal cashback now fully overrides default (previously allowed merge)
- `accountId` field in `POST /cashback-histories/withdraw` became optional (previously required)

### Fixed
- 500 when adding personal cashbacks via backend
- Incorrect logout and missing redirect to login page
- Incorrect display of % payment fields in default cashbacks on validation errors
- LDAP: missing group in Keycloak and unavailable connection check on update
- Missing transactions in user analytics and transaction import errors
- Terminal import errors
- Incorrect endDate when creating cashback

## [0.3.0] - 2026-02-03

### Added
- Vault integration with environments, services, secret import and documentation
- Deeployalty integration - uploading merchants, terminals and cashback programs
- Kafka library, connection manager and consumer management
- DataSources and DataSourceMappers modules with CRUD, test endpoints and schema analyzer
- Complete Webhooks stack: subscriptions, mappers, events, delivery logs and payload templates
- Keycloak configuration module with LDAP endpoints, groups/roles and statistics
- MinIO module and bucket/policy initialization scripts
- Mobile access keys module and endpoints

### Changed
- CI/CD pipelines and GitHub Actions for building/publishing images with version tag
- Dockerfile and docker-compose for local and prod environments, nginx/filebeat configs
- Dynamic DB: TS seeds, new migrations, bulk-json-save and SQL helpers
- [DB] Schema and migration updates (tables/columns/indexes):
  - New tables:
    - `DATA_SOURCES`
    - `DATA_SOURCE_MAPPERS`
  - `TRANSACTIONS`:
    - added `DATA_SOURCE_ID` column (relation to `DATA_SOURCES`)
  - `CUSTOMER`:
    - added `ADDED_BONUSES` column (decimal, default 0)
  - `RECEIPTS`:
    - removed retry columns: `RETRY_ATTEMPTS`, `NEXT_RETRY_AT`, `RETRY_ERROR_MESSAGE`, `RETRY_STATUS`
    - removed index `IDX_RECEIPTS_RETRY_STATUS_NEXT_RETRY` (on `RETRY_STATUS`, `NEXT_RETRY_AT`)
  - Webhooks (table schema):
    - `WEBHOOK_SUBSCRIPTIONS`: added authorization/delivery fields (`HTTP_METHOD`, `USE_AUTHORIZATION`, `AUTH_TYPE`, `AUTH_API_KEY`, `AUTH_LOCATION`, `AUTH_FIELD_NAME`)
    - `WEBHOOK_SUBSCRIPTIONS`: added unique index `IDX_WEBHOOK_SUBSCRIPTIONS_CALLBACK_URL` (on `CALLBACK_URL`)
    - `WEBHOOK_MAPPERS`: removed columns `AUTHORIZATION_TYPE`, `HTTP_METHOD` (moved to `WEBHOOK_SUBSCRIPTIONS`)
    - `WEBHOOK_EVENTS`: `DESCRIPTION` → `DESCRIPTION_EN` + added `NAME_EN`, `NAME_UA`, `DESCRIPTION_UA` (multilang)
  - Migrations:
    - many old migrations removed/consolidated, added new targeted migrations for changes above
- Logging and error handling in key services (centralized handlers)
- Transaction and cashback processing: SQL operations, batch/parallel, flexible dates
- Keycloak/Redis/Deeployalty configurations and vault path structure for environments

### Fixed
- Reliability of initialization and logging during DB/Redis/Kafka connections

## [0.2.0] - 2025-12-03

### Added
- Logic for uploading and processing files with client ID lists for personal cashbacks
- API methods for adding and updating customer list in configured cashback

### Changed
- Card products UI changed from table view to cards
- Updated frontend and connected methods for personal cashbacks
- Changed response description in `/cashback-histories/withdraw` if user balance is less than withdrawal amount

### Fixed
- Fixed ability to delete cashback for current month

## [0.1.0] - 2025-11-17

### Added
- Implemented basic referral program
- Endpoint for uploading transactions to system
- Ability to delete cashback for next period
- Terminal search and terminal upload from file in merchant form
- Validation for forms of creating/editing groups, MCC, card products, merchants
- Additional attempts to search for transaction for receipt matching
- Restriction on adding files with terminal ID
- Reversal of accruals on add-cashback and withdraw errors

### Changed
- "Program Name" field for referral promotions is now required when creating promotional link
- Minimum amount per merchant is now required field for personal cashbacks
- In user analytics, cashback and referral program data displayed in separate menus
- Hidden unavailable dates when creating cashback
- Hidden terminal list by merchant
- [DevOps] Restricted Elasticsearch (Kibana) log retention to 3 months
- Fixed handling of cases "Current customer balance less than minimum amount for bonus withdrawal"

### Fixed
- Customer can no longer register with their own referral code
- Fixed 500 error when registering new user by referral code
- Fixed "Registration limit reached" error when changing maximum number of users for registration
- Fixed "Back" button name and section name for user program
- Edit promotional link form no longer closes when copying code
- Fixed "Registration limit reached" error when registering by promotional code
- Fixed display of table with link data
- Fixed display of invited/inviter amount in user analytics
- Blocked registration by inactive by dates promotional link
- Fixed display of user link data table on repeated section click
- Fixed refund amount overflow
- Fixed dataset when exporting to customer file for personal cashback