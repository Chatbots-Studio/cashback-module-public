# Changelog

## [0.7.2] - 2026-05-11

### Added

- Ability to delete a merchant terminal file in cashback when editing
- Ability to reprocess transactions through the "Merchants" section: "Recalculate cashback" button in the terminal editing block is activated when adding new terminals
- Logic for automatic recalculation of transactions for the current month when adding new terminals to a merchant

![Demonstration of cashback recalculation by merchant](images/changelog/all_types_recount.en.gif)

### Changed

- Updated Node.js version
- Changed the merchant form: split into two tabs — "Merchant Settings" and "Terminal Settings"; added search by merchant name and separate icons for editing settings and terminals
- Removed card type filter on all routes for getting cashback data in the mobile gateway (except co-branding)
- Changed the Kafka/webhook mapper page: updated mapper validation logic and results display
- New manual terminal addition field now appears at the top when adding more than one terminal
- Changed the name of the "Add Manually" block to "Add Terminals Manually" in the merchant form
- "Recalculate Cashback" button is now active for new merchants (independent of active cashback binding)

### Fixed

- After session timeout expired, Unauthorized Access page was displayed instead of the login page
- Operation type was not displayed in operation details in user analytics
- No ability to delete a merchant terminal file in cashback when editing merchants
- Null was automatically set in name and location fields when adding terminals to a merchant via file
- Pagination was cut off when editing terminals in the "Merchants" section
- Incorrect search by terminal ID through the search bar in the merchant listing
- Bottom highlight of the active terminal viewing page was cut off in merchant terminals
- Error when adding a previously exported merchant terminal file
- Selected terminal field for archiving was cut off
- Tooltip size did not match the text length in cashback settings
- Page title and description of the first currency rate setup page were not translated; "Currency for conversion" and "API for getting rates" fields were displayed with different heights
- Non-clickable text "Add New Link" was displayed in the promotional referral link addition form

## [0.7.1] - 2026-04-29

### Fixed

- Save button for default cashback general settings was not working: after editing settings and clicking "Save", the save request was not executed
- It was impossible to add a Deeployalty merchant to default cashback: the system returned an error updating Deeployalty merchants

## [0.6.1] - 2026-04-27

### Added

- Fixation in a separate facts table of receiving available cashbacks by user (view without selection) with webhook configuration
- Customer segmentation by merchant in default cashback: ability to set a separate cashback percentage for a segment and manage the availability of default rewards for customers outside segments
- Ability to download an example template file of terminals for segmentation setup
- Export button for the list of segment customers of a merchant
- `transactionId` field in transaction import
- E2E test execution setup within CI
- Test mode: ability to add cashbacks for different (including past) periods via ENV-parameter

### Changed

- Segment management logic moved exclusively to default cashback (removed duplication in personal cashback)
- Transaction import: added support for `cardType` field
- Transition from `TransactionProcessStatus` to `TransactionCashbackDecision`
- Clarified logic for maximum cashback amount by merchant
- Changed the "Accrual Conditions" page and "Cashback Withdrawal Conditions" section
- Unified field formats across all system forms
- Changed the name of archived parameters in all menus
- Clarified webhook settings
- Merchant settings: fixes to display and logic

### Fixed

- Cashback was not accrued for a customer from a merchant segment when all default cashback conditions were met
- Double cashback accrual on a group; missing personal cashback priority over default
- Group cashback percentage was not added to the main accrual when MCC codes matched
- Default cashback with a filled `Merchant Name` field was accrued without considering the merchant name in the transaction for the segment
- Merchant was not determined by terminal when processing a transaction
- Merchant was not determined by terminal with incorrect `merchantName`
- Merchant was not determined by terminal in the "Merchants" section
- System did not identify merchant for a valid transaction in analytics
- Default merchant cashback was not accrued by terminal in cashback
- Backend skipped merchant selection for a customer not included in default cashback
- Merchant name was not displayed in the transaction listing in analytics when reversing
- Group/merchant was not identified when insufficient minimum transaction amount for accrual
- GET `/api/v2/cashback/positions` did not return cashbacks on segments for a customer
- POST `/api/co-branded-cashback/customer` returned 400 despite active co-branded cashback
- Co-brand Web API returned empty array of selected groups and merchants
- Co-brand Mobile API: 404 when getting customer's selected positions for co-branding
- Mobile API returned incorrect customer cashback % in segment
- Mobile API (`POST /api/customer-cashback/{id}`) did not accept an array
- Mobile API incorrectly returned withdrawal value in the settings route
- Personal cashback: web gateway returned duplicates of selected merchant/group by customer in analytics
- Personal cashback was saved without uploaded customer file
- Personal cashback accepted invalid customer files
- System rejected segment customer file with valid Customer IDs
- Terminal file could not be added to cashback with a segment
- Terminal file upload in cashback completed with an error when using a template
- If a merchant had 1 terminal — it could not be deleted
- Adding segmentation added a new merchant to default cashback
- Incorrect toggle logic in default cashback segmentation
- Default cashback was not saved if the merchant minimum amount exceeded the maximum cashback amount
- 500 when deleting a merchant in the current cashback
- 500 when editing a short description to 2000+ characters
- 500 (backend crash) on stage — regression fixed
- Deeployalty duplicated a large number of default cashbacks to the next month
- Webhook was not sent to Deeployalty about receipt non-matching
- Merchant campaign terminal count from Deeployalty was not duplicated in default cashback
- Error editing merchant when adding invalid terminal values
- Editing operation type name worked incorrectly depending on language localization
- Hook was not sent to Deeployalty about receipt non-matching
- User registration error using referral code
- Incorrect calculation of registration limit for referral link
- "Last Update Date" field was not updated after adding customers to personal cashback
- No confirmation window when deleting customers from a merchant segment
- When reaching the maximum segment limit, a message "Merchant limit reached" was displayed
- Save button for default cashback general settings was not working
- "Docs" button was not working
- Localization key was displayed in the merchant segment form
- Message language did not match interface locale language
- Missing validation of amount fields in the "Cashback Withdrawal Conditions" subsection; fields allowed negative values
- Incorrect error message when withdrawing cashback (same response for different rejection scenarios in the `/withdraw` route)
- It was possible to delete Card Product from default cashback of future period without error
- File import without card type was not working
- `Cannot GET` errors when loading cashback page
- Page scrolled to header when switching pagination
- Layout "floated" depending on the number of digits in pagination
- Centered tooltip relative to row in transaction import details
- Missing asterisk for mandatory field next to "Group" field in each cashback type
- "Cashback" item in side menu had incorrect text alignment and displayed unnecessary tooltip on hover
- Lexical error in default cashback settings
- Set favicon on changelog page
- Granted archiving rights to business admin

## [0.6.0] - 2026-03-25

### Added

- Multi-currency support: currency exchange rate reference (NBU), automatic transaction conversion to system currency, saving conversion data (original amount, currency, rate) together with transaction
- Separate `INVALID_TRANSACTIONS` table for invalid transactions (missing currency, unknown rate, etc.) with reason display in user analytics
- New cashback type — co-branded: support for card type settings, parallel work with default cashback, separate mobile endpoints for getting settings and positions
- `Merchant Name` field as an additional condition for matching transactions by merchant (together with Terminal ID)
- Additional `terminal_name` and `terminal_location` fields when adding terminals by merchant
- Counter of total uploaded terminals for each merchant across all cashback types
- Endpoint for getting cashback data for a specific transaction (search by transaction ID)
- Paginated endpoint for getting complete cashback history of a user (duplicated to mobile gateway)
- `bannerUrl` field in mobile endpoint `/cashbacks/positions` (only for merchant type positions)
- `accountId` field in `CASHBACK_HISTORY` — stores the account to which funds were withdrawn
- Return `cashbackHistoryId` in response of `/withdraw` endpoint (to support reversal operations)
- [Deeployalty] Sending webhook about customer's selected cashback to Deeployalty when changing customer settings
- Deployment of cashback module for "Clearing House" client
- Display of Deeployalty note on merchants when selecting them during cashback creation
- Ability to view errors by rows when importing transactions
- Display of mandatory fields in all system forms

### Changed

- [Deeployalty] Updated terminal synchronization logic: terminals in "Merchants" section are only supplemented (not deleted), in merchant cashback block — current terminals of current campaign
- Frontend analytics displays transaction dates in local time (instead of UTC)
- `accountId` field in `/cashback-histories/withdraw` endpoint became mandatory
- Statistics page is hidden (data was hardcoded, did not display real values)
- Unified pagination across all tabs with table display

### Fixed

- Unable to change settings for Deeployalty merchant for next month cashback
- Cashback was not reduced during refund with currency conversion
- 500 error when withdrawing cashback
- 500 error when withdrawing funds for "Clearing House" client
- Authorization error on Oschadbank server (dev)
- Database connection error (dev Oschadbank)
- Co-branded cashback accrual occurred without considering card type
- Active co-branded cashback for current month available for editing
- No ability to disable currency conversion during initial system setup
- "Save" button was active even without changes in currency API settings
- UAH transaction passed only in uppercase (UPPERCASE)
- Oracle: `IN()` constraint on 1000 elements — fixed with batch processing
- After logout, redirect to `/setup-database` instead of `/login`
- Missing redirect to login page after logout confirmation in Settings section
- Column names `CreatedAt`, `Parent ID`, `LDAP Filter` were not translated to Ukrainian in respective sections
- Currency names in "Currency Rate" section were displayed without country name in Ukrainian localization
- Amount in transaction list was displayed without conversion to system currency
- "Documentation" button opened home page instead of documentation
- Merchant terminal file was downloaded empty
- Terminal counter in merchant card counted terminals from cashback settings
- Errors when working with WebPlatform API (Clearing House)

## [0.5.0] - 2026-02-26

### Added
- Time zone configuration for correct transaction processing
- Added DB_ENCRYPT & DB_TRUST_SERVER_CERTIFICATE settings for MSSQL connection
- Built-in documentation link in admin interface
- MD-format support in "Description" field for merchant and delivery of value to API
- Kafka Enrichment Microservice: integration with Client API, transaction enrichment, Kafka message consumption
- MapperCheckService for DataSourceMapper validation before Kafka operations
- Paginated endpoint for cashback history with date filters
- TRANSACTION_FAILURES table and workflow for failed transactions
- GroupCashbackHistoryItemResponse for simplified cashback history in transaction details
- maxAmount at merchant level
- Banner URL field for merchants
- Short description for merchants and groups
- processErrorMessage in transaction processing responses

### Changed
- Changed cashback editing logic
- [Deeployalty] Fetch max cashback amount field for merchant during Deeployalty integration
- [Deeployalty] Cron runs once per hour to update cashback offers
- Removed CurrentCashbackResponse and related logic
- Refactored cashback service logic for personal/default cashbacks
- Refactored refund transaction processing

### Fixed
- Cron job overlap
- Getting cashbacks by specific type not working
- Transaction import available to non-admin
- Incorrect prod link Deeployalty for cashback integration
- Error HTTPS dev stage
- Prod council: merchants marked as from Deeployalty
- Referral program available to non-admin
- Prod council: missing previously added users in configurator
- Prod council: GET error when transitioning to webhooks section
- No ability to export list of customers for personal cashback
- Transaction import — notification message not translated when changing interface language
- Errors when working with WebPlatform API clearing house
- Blocked merchant settings fields for Deeployalty when adding to default/personal cashback
- Error adding merchant to current default cashback if it had Deeployalty merchant
- Incorrectly accrued cashback by merchant
- Personal cashback creation
- Deeployalty time zone

## [0.4.0] - 2026-02-12

### Added
- CRUD for cashback types (cashback operation types) in admin panel
- Automatic initialization of nv-keys in Vault on first run
- Import and export of terminal list for cashback by merchant
- Transaction upload section in admin panel
- Settings section in admin panel with ability to:
  - update LDAP connection and role mappers
  - manage manually created users
  - change access to edit past cashback periods
  - configure webhooks
  - add new access keys for Mobile Gateway (authorization)
  - change database settings (not recommended after initial setup)
  - add new data sources (e.g., transaction upload from Kafka)
- New endpoint `GET /api/v2/cashback/current` to get current cashback settings, positions, and selected card products in a single request

### Changed
- Webhook mapping routes with field transfer to subscriptions
- "Description" field validation for merchants: all special characters allowed
- Removed deprecated endpoints:
  - `GET /customer-cashback/chosen/:id`
  - `GET /settings`
  - `GET /allowed-cashbacks/positions`
  - `GET /receipt/transaction/:id`
- Personal cashback now fully overrides default (previously position merge occurred)
- `accountId` field in `POST /cashback-histories/withdraw` became optional (previously required)

### Fixed
- 500 when adding personal cashbacks via backend
- Incorrect logout and missing redirect to login page
- Display of % payment fields in default cashbacks on validation errors
- LDAP: missing group in Keycloak and inability to check connection when updating
- Missing transactions in user analytics and transaction import errors
- Terminal import error
- Incorrect endDate when creating cashback

## [0.3.0] - 2026-02-03

### Added
- Vault integration with environments, services, secret import, and documentation
- Deeployalty integration - upload merchants, terminals, and cashback programs
- Kafka library, connection manager, and consumer management
- DataSources and DataSourceMappers modules with CRUD, test endpoints, and schema analyzer
- Full Webhooks stack: subscriptions, mappers, events, delivery logs, and payload templates
- Keycloak configuration module with LDAP endpoints, groups/roles, and statistics
- MinIO module and bucket/policy initialization scripts
- Mobile access keys module and endpoints

### Changed
- CI/CD pipelines and GitHub Actions for building/publishing images with version tag
- Dockerfile and docker-compose for local and prod environments, nginx/filebeat configs
- Dynamic database: TS seeds, new migrations, bulk-json-save, and SQL helpers
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
    - removed index `IDX_RECEIPTS_RETRY_STATUS_NEXT_RETRY` (on `RETRY_STATUS`, `NEXT_RETRY_AT`)
  - Webhooks (table schema):
    - `WEBHOOK_SUBSCRIPTIONS`: added authorization/delivery fields (`HTTP_METHOD`, `USE_AUTHORIZATION`, `AUTH_TYPE`, `AUTH_API_KEY`, `AUTH_LOCATION`, `AUTH_FIELD_NAME`)
    - `WEBHOOK_SUBSCRIPTIONS`: added unique index `IDX_WEBHOOK_SUBSCRIPTIONS_CALLBACK_URL` (on `CALLBACK_URL`)
    - `WEBHOOK_MAPPERS`: removed `AUTHORIZATION_TYPE`, `HTTP_METHOD` columns (moved to `WEBHOOK_SUBSCRIPTIONS`)
    - `WEBHOOK_EVENTS`: `DESCRIPTION` → `DESCRIPTION_EN` + added `NAME_EN`, `NAME_UA`, `DESCRIPTION_UA` (multilang)
  - Migrations:
    - many old migrations removed/consolidated, new targeted migrations added for changes above
- Logging and error handling in key services (centralized handlers)
- Transaction and cashback processing: SQL operations, batch/parallel, flexible dates
- Keycloak/Redis/Deeployalty configurations and vault paths structure for environments

### Fixed
- Reliability of initialization and logging during connections to database/Redis/Kafka

## [0.2.0] - 2025-12-03

### Added
- Logic for uploading and processing files with list of customer IDs for personal cashbacks
- API methods for adding and updating customer list in configured cashback

### Changed
- Card product UI changed from table view to cards
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
- Validation for forms of creating/editing groups, MCCs, card products, merchants
- Additional attempts to find transaction for receipt matching
- Restriction on adding files with terminal ID
- Reversal of accruals on add-cashback and withdraw errors

### Changed
- "Program Name" field for promotional referrals now mandatory when creating promotional link
- Minimum amount by merchant now required field for personal cashbacks
- In user analytics, cashback and referral program data displayed as separate menu items
- Hidden unavailable dates when creating cashback
- Hidden list of terminals by merchant
- [DevOps] Limited log storage in Elasticsearch (Kibana) to 3 months
- Fixed handling of "Current customer balance is less than minimum withdrawal amount" cases

### Fixed
- Customer can no longer register using their own referral code
- Fixed 500 error when registering new user via referral code
- Fixed "Registration limit reached" error when changing maximum user registrations
- Fixed "Back" button name and user program section
- Promotional link editing form no longer closes when copying code
- Fixed "Registration limit reached" error when registering via promotional code
- Fixed display of table with link data
- Fixed amount display for invited/inviter in user analytics
- Blocked registration via inactive promotional link by dates
- Fixed disappearance of link data table when clicking section again
- Fixed refund overflow
- Fixed data set when exporting customers to file for personal cashback