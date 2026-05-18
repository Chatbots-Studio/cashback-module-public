# Changelog

## [0.7.2] - 2026-05-11

### Added

- Ability to delete merchant terminal file in cashback during editing
- Ability to reprocess transactions through the "Merchants" section: the "Recalculate cashback" button in the terminal editing block is activated when adding new terminals
- Logic for automatic recalculation of transactions for the current month when adding new terminals to a merchant

![Demonstration of cashback recalculation by merchant](images/changelog/all_types_recount.en.gif)

### Changed

- Updated Node.js version
- Modified merchant form: split into two tabs — "Merchant Settings" and "Terminal Settings"; added search by merchant name and separate icons for editing settings and terminals
- Removed card type filter on all routes for getting cashback data in mobile gateway (except co-branding)
- Updated Kafka/webhook mapper page: refreshed mapper verification logic and results display
- New manual terminal addition field now appears at the top when adding more than one terminal
- Changed "Add Manually" block name to "Add Terminals Manually" in the merchant form
- "Recalculate Cashback" button is now active for new merchants (not tied to active cashback)

### Fixed

- After session timeout, the Unauthorized Access page was displayed instead of the login page
- Operation type was not displayed in operation details in user analytics
- Unable to delete merchant terminal file in cashback when editing merchants
- Null was automatically set in name and location fields when adding terminals to a merchant by file
- Pagination was cut off when editing terminals in the "Merchants" section
- Incorrect search by Terminal ID through the search field in merchant listing
- Active page highlight was cut off at the bottom in terminal viewing for a merchant
- Error when adding previously exported merchant terminal file
- Selected terminal field for archiving was cut off
- Tooltip size did not match text length in it in cashback settings
- Page title and description of the first currency rate setup page were not translated; "Currency for Conversion" and "API for Getting Rates" fields displayed with different heights
- Non-clickable text "Add New Link" was displayed on the promotional referral link addition form

## [0.7.1] - 2026-04-29

### Fixed

- Default cashback general settings save button did not work: after editing settings and clicking "Save", the save request was not executed
- Unable to add Deeployalty merchant to default cashback: system returned an error updating Deeployalty merchants

## [0.6.1] - 2026-04-27

### Added

- Fixation in a separate fact table of receiving available cashbacks by user (viewing without selection) with webhook configuration
- Client segmentation by merchant in default cashback: ability to set separate cashback percentage for segment and manage availability of default rewards for clients outside segments
- Ability to download example terminal template file for segmentation configuration
- Export button for segment merchant client list
- `transactionId` field in transaction import
- E2E test run configuration within CI
- Test mode: ability to add cashbacks for different (including past) periods via ENV parameter

### Changed

- Segment management logic moved exclusively to default cashback (removed duplication in personal)
- Transaction import: added support for `cardType` field
- Transition from `TransactionProcessStatus` to `TransactionCashbackDecision`
- Clarified maximum cashback amount logic by merchant
- Updated "Accrual Conditions" page and "Cashback Withdrawal Conditions" section
- Unified field formats across all system forms
- Changed archived parameters naming in all menus
- Clarified webhook settings
- Merchant settings: corrections to display and logic

### Fixed

- Cashback was not accrued for client from merchant segment when meeting all default cashback conditions
- Double cashback accrual by group; missing priority of personal cashback over default
- Group cashback percentage was not added to main accrual when MCC code matched
- Default cashback with filled `Merchant Name` field was accrued without considering merchant name in transaction by segment
- Merchant was not identified by terminal during transaction processing
- Merchant was not identified by terminal with incorrect `merchantName`
- Merchant was not identified by terminal in "Merchants" section
- System did not identify merchant for valid transaction in analytics
- Default merchant cashback by terminal was not accrued in cashback
- Backend skipped merchant selection for client not included in default cashback
- Merchant name was not displayed in transaction listing in analytics during reversal
- Group/merchant was not identified with insufficient minimum transaction amount for accrual
- GET `/api/v2/cashback/positions` did not return cashbacks by segments for client
- POST `/api/co-branded-cashback/customer` returned 400 despite active co-branded cashback
- Co-branded Web API returned empty array of selected groups and merchants
- Co-branded Mobile API: 404 when getting selected client positions by co-branding
- Mobile API returned incorrect % cashback for client in segment
- Mobile API (`POST /api/customer-cashback/{id}`) did not accept array
- Mobile API incorrectly returned withdrawal value in settings route
- Personal cashback: web gateway returned duplicates of selected merchant/group by client in analytics
- Personal cashback was saved without uploaded client file
- Personal cashback accepted invalid client files
- System rejected segment client file with valid Customer ID
- Terminal file was not added to cashback with segment
- Terminal file upload in cashback failed when using template
- If merchant had 1 terminal — it could not be deleted
- Adding segmentation added new merchant to default cashback
- Incorrect toggle logic in default cashback segmentation
- Default cashback was not saved if merchant minimum amount was greater than maximum cashback amount
- 500 when deleting merchant in current cashback
- 500 when editing short description to 2000+ characters
- 500 (backend crash) on stage — regression removed
- Deeployalty duplicated large number of default cashbacks to next month
- Webhook was not sent to Deeployalty about receipt NO-match
- Merchant campaign terminal count from Deeployalty was not duplicated in default cashback
- Error editing merchant when adding invalid terminal values
- Operation type name editing worked incorrectly depending on language localization
- Hook was not sent to Deeployalty about receipt NO-match
- Error in user registration by referral code
- Incorrect calculation of registration limit by referral link
- "Last Update Date" field was not updated after adding clients to personal cashback
- Confirmation window was missing when deleting segment clients of merchant
- Upon reaching maximum segment limit, message "Merchant Limit Reached" was displayed
- Default cashback general settings save button did not work
- "Docs" button did not work
- Localization key was displayed in merchant segment form
- Message language did not match interface locale language
- Fields amount validation was missing in "Cashback Withdrawal Conditions" subsection; fields allowed entering negative values
- Incorrect error message when withdrawing cashback (same response for different denial scenarios in `/withdraw` route)
- Possibility to delete Card Product from default cashback for future period without error
- File import without card type did not work
- `Cannot GET` errors when loading cashback page
- Page scrolled to header when switching pagination
- Layout "floats" depending on number of digits in pagination
- Tooltip centered relative to line in transaction import details
- Missing asterisk for required field next to "Group" field in each cashback type
- "Cashback" menu item in sidebar had incorrect text alignment and displayed extra tooltip on hover
- Lexical error in default cashback settings
- Favicon added to changelog page
- Business admin given archiving rights

## [0.6.0] - 2026-03-25

### Added

- Multi-currency support: currency rate reference (NBU), automatic transaction conversion to system currency, saving conversion data (original amount, currency, rate) along with transaction
- Separate `INVALID_TRANSACTIONS` table for invalid transactions (missing currency, unknown rate, etc.) with reason display in user analytics
- New cashback type — co-branded: support for settings by card type, parallel work with default cashback, separate mobile endpoints for getting settings and positions
- Field `Merchant Name` as additional condition for transaction matching by merchant (together with Terminal ID)
- Additional fields `terminal_name` and `terminal_location` when adding terminals to merchant
- Counter of total downloaded terminals for each merchant across all cashback types
- Endpoint for getting cashback data for specific transaction (search by transaction ID)
- Paginated endpoint for getting complete cashback history of user (duplicated to mobile gateway)
- Field `bannerUrl` in mobile endpoint `/cashbacks/positions` (only for merchant type positions)
- Field `accountId` in `CASHBACK_HISTORY` — stores account to which funds were withdrawn
- Return of `cashbackHistoryId` in response of `/withdraw` endpoint (for supporting reversal operations)
- [Deeployalty] Sending webhook about selected user cashback to Deeployalty when changing customer settings
- Deployment of cashback module for "Clearing House" client
- Display of Deeployalty mark on merchants when selecting them during cashback creation
- Ability to view errors by lines when importing transactions
- Display of required fields in all system forms

### Changed

- [Deeployalty] Updated terminal synchronization logic: terminals in "Merchants" section only supplemented (not deleted), in merchant cashback block — current terminals of current campaign
- Frontend analytics displays transaction dates in local time (instead of UTC)
- Field `accountId` in `/cashback-histories/withdraw` endpoint became required
- Statistics page hidden (data was hardcoded, did not display real values)
- Unified pagination on all tabs with tabular display

### Fixed

- Unable to change Deeployalty merchant settings for cashback for next month
- Cashback was not subtracted for refund with currency conversion
- 500 error when withdrawing cashback
- 500 error when withdrawing funds for "Clearing House" client
- Authorization error on Oschadbank server (dev)
- Database connection error (dev Oschadbank)
- Co-branded cashback accrual did not account for card type
- Active co-branded cashback for current month available for editing
- Unable to disable currency conversion during initial system setup
- "Save" button was active even without changes in currency API settings
- UAH transaction passed only in uppercase (UPPERCASE)
- Oracle: `IN()` limitation of 1000 elements — fixed with batch processing
- After logging out, redirect to `/setup-database` instead of `/login`
- Missing redirect to login page after confirming logout in Settings section
- Column names `CreatedAt`, `Parent ID`, `LDAP Filter` were not translated to Ukrainian in respective sections
- Currency names in "Currency Rate" section displayed without country name in Ukrainian localization
- Amount in transaction list displayed without conversion to system currency
- "Documentation" button opened main page instead of documentation
- Merchant terminal file exported empty
- Terminal counter in merchant card accounted for terminals from cashback settings
- Errors when working with WebPlatform API (Clearing House)

## [0.5.0] - 2026-02-26

### Added
- Timezone settings for correct transaction processing
- Added DB_ENCRYPT & DB_TRUST_SERVER_CERTIFICATE settings for MSSQL connection
- Documentation link embedded in admin interface
- Support for MD-format in "Description" field by merchant and value transfer to API
- Kafka Enrichment Microservice: integration with Client API, transaction enrichment, Kafka message consumption
- MapperCheckService for DataSourceMapper validation before Kafka operations
- Paginated cashback history endpoint with date filters
- TRANSACTION_FAILURES table and workflow for failed transactions
- GroupCashbackHistoryItemResponse for simplified cashback history in transaction details
- maxAmount at merchant level
- Banner URL field for merchants
- Short description for merchants and groups
- processErrorMessage in transaction processing responses

### Changed
- Logic change for cashback editing
- [Deeployalty] Retrieval of max cashback amount by merchant when integrating Deeployalty
- [Deeployalty] Cron run once per hour to update cashback offers
- Removed CurrentCashbackResponse and related logic
- Refactored cashback service logic for personal/default cashbacks
- Refactored refund transaction processing

### Fixed
- Overlapping cron jobs
- Cashback retrieval by certain type did not work
- Transaction import available to non-admin
- Incorrect prod link Deeployalty for cashback integration
- HTTPS dev stage error
- Rada PROD: merchants marked as from Deeployalty
- Referral program available to non-admin
- Prod rada: previously added users missing in configurator
- Prod rada: GET error when navigating to webhooks section
- Unable to export customer list by personal cashback
- Transaction import — popup message not translated when changing interface language
- Errors when working with WebPlatform API clearing house
- Deeployalty merchant settings fields blocked when adding to default/personal cashback
- Error adding merchant to current default cashback if it contained Deeployalty merchant
- Incorrectly accrued cashback by merchant
- Personal cashback creation
- Deeployalty timezone

## [0.4.0] - 2026-02-12

### Added
- CRUD for cashback operation types in admin
- Automatic NV-key initialization in Vault on first run
- Import and export of terminal list for merchant cashback
- Transaction upload section in admin
- Settings section in admin with ability to:
  - update LDAP connection and role mappers
  - manage manually created users
  - change access rights to edit past cashback periods
  - configure webhooks
  - add new access keys for Mobile Gateway (authorization)
  - change database settings (not recommended after initial setup)
  - add new data sources (e.g., transaction upload from Kafka)
- New endpoint `GET /api/v2/cashback/current` for getting current cashback settings, positions and selected card products in one request

### Changed
- Webhook mapping routes with field transfer to subscriptions
- Validation of "Description" field in merchants: all special characters allowed
- Removed deprecated endpoints:
  - `GET /customer-cashback/chosen/:id`
  - `GET /settings`
  - `GET /allowed-cashbacks/positions`
  - `GET /receipt/transaction/:id`
- Personal cashback now completely overrides default (previously merge of allowed positions occurred)
- Field `accountId` in `POST /cashback-histories/withdraw` became optional (previously required)

### Fixed
- 500 when adding personal cashbacks via backend
- Incorrect logout and missing redirect to authorization page
- Display of % payment fields in default cashbacks when validation errors
- LDAP: missing group in Keycloak and unavailable connection check when updating
- Missing transactions in user analytics and transaction import errors
- Terminal import error
- Incorrect endDate when creating cashback

## [0.3.0] - 2026-02-03

### Added
- Vault integration with environments, services, secret import and documentation
- Deeployalty integration - upload merchants, terminals and cashback programs
- Kafka library, connection manager and consumer management
- DataSources and DataSourceMappers modules with CRUD, test endpoints and schema analyzer
- Complete Webhooks stack: subscriptions, mappers, events, delivery logs and payload templates
- Keycloak configuration module with LDAP endpoints, groups/roles and statistics
- MinIO module and bucket/policy initialization scripts
- Mobile access keys module and endpoints

### Changed
- CI/CD pipelines and GitHub Actions for image building/publishing with version tag
- Dockerfile and docker-compose for local and prod environments, nginx/filebeat configs
- Dynamic DB: TS seeds, new migrations, bulk-json-save and SQL helpers
- [DB] Schema and migration updates (tables/columns/indexes):
  - New tables:
    - `DATA_SOURCES`
    - `DATA_SOURCE_MAPPERS`
  - `TRANSACTIONS`:
    - added `DATA_SOURCE_ID` column (link to `DATA_SOURCES`)
  - `CUSTOMER`:
    - added `ADDED_BONUSES` column (decimal, default 0)
  - `RECEIPTS`:
    - removed retry columns: `RETRY_ATTEMPTS`, `NEXT_RETRY_AT`, `RETRY_ERROR_MESSAGE`, `RETRY_STATUS`
    - removed index `IDX_RECEIPTS_RETRY_STATUS_NEXT_RETRY` (by `RETRY_STATUS`, `NEXT_RETRY_AT`)
  - Webhooks (table schema):
    - `WEBHOOK_SUBSCRIPTIONS`: added authorization/delivery fields (`HTTP_METHOD`, `USE_AUTHORIZATION`, `AUTH_TYPE`, `AUTH_API_KEY`, `AUTH_LOCATION`, `AUTH_FIELD_NAME`)
    - `WEBHOOK_SUBSCRIPTIONS`: added unique index `IDX_WEBHOOK_SUBSCRIPTIONS_CALLBACK_URL` (by `CALLBACK_URL`)
    - `WEBHOOK_MAPPERS`: removed columns `AUTHORIZATION_TYPE`, `HTTP_METHOD` (moved to `WEBHOOK_SUBSCRIPTIONS`)
    - `WEBHOOK_EVENTS`: `DESCRIPTION` → `DESCRIPTION_EN` + added `NAME_EN`, `NAME_UA`, `DESCRIPTION_UA` (multilang)
  - Migrations:
    - many old migrations removed/consolidated, new point migrations added for changes above
- Logging and error handling in key services (centralized handlers)
- Transaction and cashback processing: SQL operations, batch/parallel, flexible dates
- Keycloak/Redis/Deeployalty configurations and vault path structure for environments

### Fixed
- Reliability of initialization and logging during DB/Redis/Kafka connections

## [0.2.0] - 2025-12-03

### Added
- Logic for uploading and processing files with customer ID list for personal cashbacks
- API methods for adding and updating customer list in configured cashback

### Changed
- Card product UI changed from tabular view to cards
- Updated frontend and connected personal cashback methods
- Updated `/cashback-histories/withdraw` response description if user balance is less than withdrawal amount

### Fixed
- Fixed ability to delete cashback for current month

## [0.1.0] - 2025-11-17

### Added
- Implemented basic referral program
- Endpoint for uploading transactions to system
- Ability to delete cashback for next period
- Terminal search and terminal upload from file in merchant form
- Validation for group, MCC, card product, merchant creation/editing forms
- Additional attempts to find transaction for receipt matching
- Restrictions on adding files with terminal ID
- Reversal of accruals on add-cashback and withdraw errors

### Changed
- "Program Name" field for referral promotions now required when creating promotional link
- Minimum amount by merchant now required field for personal cashbacks
- In user analytics, cashback and referral program data displayed in separate menus
- Hidden unavailable dates when creating cashback
- Hidden terminal list by merchant
- [DevOps] Limited log storage in Elasticsearch (Kibana) to 3 months
- Fixed processing of "Current client balance less than minimum amount for bonus withdrawal" cases

### Fixed
- Customer can no longer register with their own referral code
- Fixed 500 error when registering new user by referral code
- Fixed "Registration Limit Reached" error when changing maximum user registrations per referral
- Fixed "Back" button name and user program section
- Edit promotional link form no longer closes when copying code
- Fixed "Registration Limit Reached" error when registering by promotional code
- Fixed table display with link data
- Fixed display of invited/inviter amount in user analytics
- Blocked registration by promotional link inactive by dates
- Fixed table disappearance when re-clicking link section
- Fixed incorrect refund amount
- Fixed customer export file data set by personal cashback