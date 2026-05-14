# Changelog

## [0.7.2] - 2026-05-11

### Added

- Ability to delete merchant terminal file in cashback when editing
- Ability to reprocess transactions through the "Merchants" section: the "Recalculate cashback" button in the terminal editing block is activated when adding new terminals
- Logic for automatic recalculation of transactions for the current month when adding new terminals to a merchant

![Demonstration of cashback recalculation by merchant](images/changelog/all_types_recount.gif)

### Changed

- Updated Node.js version
- Changed merchant form: split into two tabs — "Merchant Settings" and "Terminal Settings"; added search by merchant name and separate icons for editing settings and terminals
- Removed card type filter on all routes for getting cashback data in mobile gateway (except co-branding)
- Changed mapper page for Kafka/webhooks: updated mapper validation logic and result display
- New field for manual terminal addition now displays at the top when adding more than one terminal
- Changed "Add Manually" block name to "Add Terminals Manually" in the merchant form
- "Recalculate cashback" button is now active for new merchants (independent of active cashback binding)

### Fixed

- After session timeout, the Unauthorized Access page was displayed instead of the login page
- Operation type was not displayed in operation details in user analytics
- Missing ability to delete merchant terminal file in cashback when editing merchants
- Null was automatically set in name and location fields when adding terminals to merchant via file
- Pagination was cut off when editing terminals in the "Merchants" section
- Incorrect search by terminal ID through the search field in merchant listing
- Bottom highlight of active terminal viewing page was cut off
- Error when adding previously exported merchant terminal file
- Selected terminal field for archiving was cut off
- Tooltip size did not match text length in cashback settings
- Page title and description of initial currency rate setup page were not translated; fields "Currency for conversion" and "API for getting rates" were displayed with different heights
- Non-clickable text "Add new link" was displayed on the promotional referral link creation form

## [0.7.1] - 2026-04-29

### Fixed

- Save button for general default cashback settings did not work: after editing settings and pressing "Save", the save request was not executed
- Unable to add Deeployalty merchant to default cashback: the system returned an error updating Deeployalty merchants

## [0.6.1] - 2026-04-27

### Added

- Fixation of available cashbacks received by user (view without selection) to a separate fact table with webhook configuration
- Customer segmentation by merchant in default cashback: ability to set a separate cashback percentage for a segment and manage availability of default rewards for customers outside segments
- Ability to download an example template file of terminals for segmentation configuration
- Export button for merchant segment customer list
- `transactionId` field in transaction import
- Configuration for running e2e tests as part of CI
- Test mode: ability to add cashbacks for different (including past) periods via ENV parameter

### Changed

- Segment management logic moved exclusively to default cashback (removed duplication in personal)
- Transaction import: added support for `cardType` field
- Transition from `TransactionProcessStatus` to `TransactionCashbackDecision`
- Refined logic for maximum cashback amount per merchant
- Changed "Accrual Conditions" page and "Cashback Withdrawal Conditions" section
- Unified field formats across all system forms
- Changed name of archived parameters in all menus
- Refined webhook configurations
- Merchant settings: updates to display and logic

### Fixed

- Cashback was not accrued for customer from merchant segment when all default cashback conditions were met
- Double cashback accrual per group; missing priority of personal cashback over default
- Group cashback percentage was not added to main accrual when MCC codes matched
- Default cashback with filled `Merchant Name` field was accrued without considering merchant name in transaction by segment
- Merchant was not identified by terminal when processing transaction
- Merchant was not identified by terminal with incorrect `merchantName`
- Merchant was not identified by terminal in the "Merchants" section
- System did not identify merchant for valid transaction in analytics
- Default cashback was not accrued by merchant by terminal in cashback
- Backend skipped merchant selection for customer not included in default cashback
- Merchant name was not displayed in transaction list of analytics during reversal
- Group/merchant was not identified when insufficient minimum transaction amount for accrual
- GET `/api/v2/cashback/positions` did not return cashbacks by segments for customer
- POST `/api/co-branded-cashback/customer` returned 400 despite active co-branded cashback
- Co-brand. Web API returned empty array of selected groups and merchants
- Co-brand. Mobile API: 404 when getting customer's selected positions by co-branding
- Mobile API returned incorrect % cashback for customer in segment
- Mobile API (`POST /api/customer-cashback/{id}`) did not accept array
- Mobile API incorrectly returned withdrawal value in settings route
- Personal cashback: web gateway returned duplicates of selected merchant/group per customer in analytics
- Personal cashback was saved without uploaded customer file
- Personal cashback accepted invalid customer files
- System rejected customer segment file with valid Customer ID
- Unable to add terminal file to cashback with segment
- Terminal file upload in cashback failed when using template
- If merchant had 1 terminal — it was impossible to delete it
- Adding segmentation added new merchant to default cashback
- Incorrect logic of toggle operation in default cashback segmentation
- Default cashback was not saved if merchant minimum amount was greater than cashback maximum amount
- 500 when deleting merchant in current cashback
- 500 when editing short description for 2000+ characters
- 500 (backend crash) on stage — regression fixed
- Deeployalty duplicated large number of default cashbacks to next month
- Webhook about receipt NON-matching was not sent to Deeployalty
- Terminal count of merchant campaign from Deeployalty was not duplicated in default cashback
- Error editing merchant when adding invalid terminal values
- Editing operation type name worked incorrectly depending on language localization
- Hook about receipt NON-matching not sent to Deeployalty
- Error registering user by referral code
- Incorrect calculation of registration limit by referral link
- "Last update date" field was not updated after adding customers to personal cashback
- Missing confirmation window when deleting merchant segment customers
- When reaching maximum segment limit, message "Merchant limit reached" was displayed
- Save button for general default cashback settings did not work
- "Docs" button did not work
- Localization key was displayed in merchant segment form
- Message language did not match interface locale language
- Missing field validation in "Cashback Withdrawal Conditions" subsection; fields allowed entry of negative values
- Incorrect error message when withdrawing cashback (same response for different denial scenarios in `/withdraw` route)
- Ability to delete Card Product from default cashback of future period without error
- File import without card type did not work
- `Cannot GET` errors when loading cashback page
- Page scrolled to header when switching pagination
- Layout "floats" depending on number of digits in pagination
- Centered tooltip relative to line in transaction import details
- Missing asterisk of mandatory field next to "Group" field in each type of cashback
- "Cashback" item in side menu had incorrect text alignment and displayed unnecessary tooltip on hover
- Lexical error in default cashback settings
- Favicon installed on changelog page
- Business admin granted archiving rights

## [0.6.0] - 2026-03-25

### Added

- Multi-currency support: currency rate reference (NBU), automatic transaction conversion to system currency, preservation of conversion data (original amount, currency, rate) along with transaction
- Separate `INVALID_TRANSACTIONS` table for invalid transactions (missing currency, unknown rate, etc.) with reason display in user analytics
- New cashback type — co-branded: support for card type settings, parallel operation with default cashback, separate mobile endpoints for getting settings and positions
- Field `Merchant Name` as additional condition for transaction matching by merchant (along with Terminal ID)
- Additional fields `terminal_name` and `terminal_location` when adding terminals by merchant
- Counter of total uploaded terminals for each merchant across all cashback types
- Endpoint for getting cashback data for specific transaction (search by transaction ID)
- Paginated endpoint for getting complete customer cashback history (duplicated on mobile gateway)
- Field `bannerUrl` in mobile endpoint `/cashbacks/positions` (only for merchant type positions)
- Field `accountId` in `CASHBACK_HISTORY` — stores account to which funds were withdrawn
- Return of `cashbackHistoryId` in response of `/withdraw` endpoint (to support reversal operations)
- [Deeployalty] Sending webhook about selected cashback to Deeployalty when changing customer settings
- Deployment of cashback module for "Clearing House" client
- Display of Deeployalty badge on merchants when selecting them during cashback creation
- Ability to view errors by rows when importing transactions
- Display of mandatory fields in all system forms

### Changed

- [Deeployalty] Updated terminal synchronization logic: terminals in "Merchants" section are only added (not deleted), in merchant cashback block — current terminals of current campaign
- Frontend analytics displays transaction dates in local time (instead of UTC)
- Field `accountId` in `/cashback-histories/withdraw` endpoint became mandatory
- Statistics page hidden (data was hardcoded and did not reflect actual values)
- Unified pagination across all tabs with table display

### Fixed

- Unable to change Deeployalty merchant settings for cashback for next month
- Cashback was not deducted during refund with currency conversion
- 500 error when withdrawing cashback
- 500 error when withdrawing funds for "Clearing House" customer
- Authorization error on Oschadbank server (dev)
- Database connection error (dev Oschadbank)
- Co-branded cashback accrual occurred without considering card type
- Active co-branded cashback for current month available for editing
- Missing ability to disable currency conversion during initial system setup
- "Save" button was active even without changes in currency API settings
- UAH transaction only passed in uppercase (UPPERCASE)
- Oracle: `IN()` limit of 1000 elements — fixed with batch processing
- After logout, redirect occurred to `/setup-database` instead of `/login`
- Missing redirect to login page after confirming logout in Settings section
- Column names `CreatedAt`, `Parent ID`, `LDAP Filter` were not translated into Ukrainian in respective sections
- Currency names in "Currency Rate" section were displayed without country name in Ukrainian localization
- Amount in transaction list was displayed without conversion to system currency
- "Documentation" button opened main page instead of documentation
- Merchant terminal file was exported empty
- Terminal counter in merchant card counted terminals from cashback settings
- Errors when working with WebPlatform API (Clearing House)

## [0.5.0] - 2026-02-26

### Added
- Time zone configuration for correct transaction processing
- Added DB_ENCRYPT & DB_TRUST_SERVER_CERTIFICATE settings for MSSQL connection
- Integration of documentation link into admin interface
- Support for MD format in "Description" field for merchant and value return on API
- Kafka Enrichment Microservice: integration with Client API, transaction enrichment, message consumption from Kafka
- MapperCheckService for DataSourceMapper validation before Kafka operations
- Paginated endpoint for cashback history with date filters
- TRANSACTION_FAILURES table and workflow for failed transactions
- GroupCashbackHistoryItemResponse for simplified cashback history in transaction details
- maxAmount at merchant level
- Banner URL field for merchants
- Short description for merchants and groups
- processErrorMessage in transaction processing responses

### Changed
- Change of cashback editing logic
- [Deeployalty] Retrieval of maximum cashback amount per merchant field during Deeployalty integration
- [Deeployalty] Cron launch once per hour to update cashback offers
- Removed CurrentCashbackResponse and related logic
- Refactoring of cashback service logic for personal/default cashbacks
- Refactoring of refund transaction processing

### Fixed
- Cron job overlapping
- Getting cashbacks by certain type not working
- Transaction import available to non-admin
- Incorrect prod Deeployalty link for cashback integration
- HTTPS dev stage error
- Rada PROD: merchants marked as from Deeployalty
- Referral program available to non-admin
- Prod rada: missing previously added users in configurator
- Prod rada: GET error when navigating to webhooks section
- Missing ability to export customer list by personal cashback
- Transaction import — toast message not translated when changing interface language
- Errors when working with WebPlatform API clearing house
- Merchant settings fields blocked when adding Deeployalty merchant to default/personal cashback
- Error adding merchant to current default cashback if it contained Deeployalty merchant
- Incorrect cashback accrual by merchant
- Creating personal cashback
- Deeployalty time zone

## [0.4.0] - 2026-02-12

### Added
- CRUD for cashback operation types in admin
- Automatic initialization of nv-keys in Vault on first launch
- Import and export of terminal list for cashback by merchant
- Transaction upload section in admin
- Settings section in admin with ability to:
  - update LDAP connection and role mappers
  - manage manually created users
  - change access to edit past cashback periods
  - configure webhooks
  - add new access keys for Mobile Gateway (authorization)
  - change database settings (not recommended after initial setup)
  - add new data sources (e.g., transaction upload from Kafka)
- New endpoint `GET /api/v2/cashback/current` for getting current cashback settings, positions and selected card products in single request

### Changed
- Routes for webhook mapping with field movement to subscriptions
- Validation of "Description" field in merchants: all special characters allowed
- Removed deprecated endpoints:
  - `GET /customer-cashback/chosen/:id`
  - `GET /settings`
  - `GET /allowed-cashbacks/positions`
  - `GET /receipt/transaction/:id`
- Personal cashback now fully overrides default (previously allowed merge of positions)
- Field `accountId` in `POST /cashback-histories/withdraw` became optional (previously mandatory)

### Fixed
- 500 when adding personal cashbacks via backend
- Incorrect logout and missing redirect to login page
- Display of % payment fields in default cashbacks with validation errors
- LDAP: missing group in Keycloak and inability to verify connection when updating
- Missing transactions in user analytics and transaction import errors
- Terminal import error
- Incorrect endDate when creating cashback

## [0.3.0] - 2026-02-03

### Added
- Vault integration with environments, services, secret imports and documentation
- Deeployalty integration - downloading merchants, terminals and cashback programs
- Kafka library, connection manager and consumer management
- DataSources and DataSourceMappers modules with CRUD, test endpoints and schema analyzer
- Complete Webhooks stack: subscriptions, mappers, events, delivery logs and payload templates
- Keycloak configuration module with LDAP endpoints, groups/roles and statistics
- MinIO module and bucket/policy initialization scripts
- Mobile access keys module and endpoints

### Changed
- CI/CD pipelines and GitHub Actions for building/publishing images with version tag
- Dockerfile and docker-compose for local and prod environments, nginx/filebeat configs
- Dynamic database: TS seeds, new migrations, bulk-json-save and SQL helpers
- [DB] Database schema and migrations update (tables/columns/indices):
  - New tables:
    - `DATA_SOURCES`
    - `DATA_SOURCE_MAPPERS`
  - `TRANSACTIONS`:
    - added `DATA_SOURCE_ID` column (relationship with `DATA_SOURCES`)
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
- Reliability of initialization and logging during connections to DB/Redis/Kafka

## [0.2.0] - 2025-12-03

### Added
- Logic for uploading and processing files with list of customer IDs for personal cashbacks
- API methods for adding and updating customer list in configured cashback

### Changed
- Card products UI changed from table view to cards
- Updated frontend and connected methods for personal cashbacks
- Changed description of response in `/cashback-histories/withdraw` if customer balance is less than withdrawal amount

### Fixed
- Fixed ability to delete cashback for current month

## [0.1.0] - 2025-11-17

### Added
- Implemented basic referral program
- Endpoint for uploading transactions to system
- Ability to delete cashback for next period
- Terminal search and terminal upload from file in merchant form
- Validation for forms of creating/editing groups, MCC, card products, merchants
- Additional attempts to search for transaction to match receipt
- Restriction on adding files with terminal ID
- Reversal of accruals on add-cashback and withdraw errors

### Changed
- Field "Program Name" for referral campaigns is now mandatory when creating promotional link
- Minimum amount per merchant is now mandatory field for personal cashbacks
- In user analytics, cashback and referral program data are displayed in separate menus
- Hidden unavailable dates when creating cashback
- Hidden terminal list by merchant
- [DevOps] Limited log storage in Elasticsearch (Kibana) to 3 months
- Fixed handling of cases "Customer's current balance less than minimum amount for bonus withdrawal"

### Fixed
- Customer can no longer register by their own referral code
- Fixed 500 error when registering new user by referral code
- Fixed "Registration limit reached" error when changing maximum number of user registrations
- Fixed "Back" button name and section for user program
- Edit promotional link form no longer closes when copying code
- Fixed "Registration limit reached" error when registering by promotional code
- Fixed display of table with link data
- Fixed display of invited/inviter amount in user analytics
- Blocked registration by inactive promotional link by dates
- Fixed display of links data table when clicking on section again
- Fixed missing webhook about receipt non-matching sent to Deeployalty
- Fixed opening calendar when clicking on inactive date clear button
- Fixed disappearance of links data table on repeated section click
- Fixed error message when registering by invalid code
- Fixed exceeding refund
- Fixed data set when exporting to file of customers by personal cashback