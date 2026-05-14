# Changelog

## [0.7.2] - 2026-05-11

### Added

- Ability to delete a merchant's terminal file in cashback during editing
- Ability to reprocess transactions through the "Merchants" section: the "Recalculate cashback" button in the terminal editing block is activated when adding new terminals
- Logic for automatic recalculation of transactions for the current month when adding new terminals to a merchant

![Demonstration of cashback recalculation by merchant](images/changelog/all_types_recount.en.gif)

### Changed

- Updated Node.js version
- Changed the merchant form: divided into two tabs — "Merchant Settings" and "Terminal Settings"; added search by merchant name and separate icons for editing settings and terminals
- Removed card type filter on all routes for retrieving cashback data in mobile gateway (except co-branding)
- Changed the mapper page for Kafka/webhooks: updated mapper verification logic and results display
- New field for manual terminal addition is now displayed at the top when adding more than one terminal
- Changed the name of the "Add Manually" block to "Add Terminals Manually" in the merchant form
- The "Recalculate cashback" button is now active for new merchants (independent of active cashback binding)

### Fixed

- After session timeout expired, an Unauthorized Access page was displayed instead of the login page
- Operation type was not displayed in operation details in user analytics
- Unable to delete a merchant's terminal file in cashback during merchant editing
- Null was automatically set in name and location fields when adding terminals to a merchant from a file
- Pagination was cut off when editing terminals in the "Merchants" section
- Incorrect search by terminal ID via search line in merchant listing
- Bottom highlighting of active merchant terminal viewing page was cut off
- Error when adding a previously exported merchant terminal file
- Selected terminal field for archiving was cut off
- Tooltip size did not match the text length in it in cashback settings
- Title and description of the first currency rate setup page were not translated; "Currency for conversion" and "API for getting rates" fields were displayed with different heights
- Non-clickable text "Add new link" was displayed on the promotional referral link addition form

## [0.7.1] - 2026-04-29

### Fixed

- Save button for general default cashback settings did not work: after editing settings and clicking "Save", the save request was not executed
- Unable to add Deeployalty merchant to default cashback: the system returned an error updating Deeployalty merchants

## [0.6.1] - 2026-04-27

### Added

- Capturing available cashbacks received by user (view without selection) into a separate fact table with webhook configuration
- Customer segmentation by merchant in default cashback: ability to set a separate cashback percentage for a segment and manage default reward availability for customers outside segments
- Ability to download a template file example for terminal configuration to set up segmentation
- Export button for segment customer list of a merchant
- `transactionId` field in transaction import
- E2E test launch configuration within CI
- Test mode: ability to add cashbacks for different (including past) periods via ENV parameter

### Changed

- Segment management logic moved exclusively to default cashback (removed duplication in personal cashback)
- Transaction import: added support for `cardType` field
- Transition from `TransactionProcessStatus` to `TransactionCashbackDecision`
- Clarified logic for maximum cashback amount per merchant
- Changed "Accrual Conditions" page and "Cashback Withdrawal Conditions" section
- Unified field formats across all system forms
- Changed the name of archived parameters in all menus
- Clarified webhook settings
- Merchant settings: display and logic fixes

### Fixed

- Cashback was not accrued for a customer from a merchant segment when all default cashback conditions were met
- Double cashback accrual per group; missing priority of personal cashback over default
- Group cashback percentage was not added to main accrual when MCC codes matched
- Default cashback with filled `Merchant Name` field was accrued without considering merchant name in transaction for segment
- Merchant was not determined by terminal during transaction processing
- Merchant was not determined by terminal with incorrect `merchantName`
- Merchant was not determined by terminal in the "Merchants" section
- System did not determine merchant for valid transaction in analytics
- Default merchant cashback by terminal was not accrued in cashback
- Backend skipped merchant selection for customer not included in default cashback
- Merchant name was not displayed in transaction listing of analytics during reversal
- Group/merchant was not determined when minimum transaction amount was insufficient for accrual
- GET `/api/v2/cashback/positions` did not return cashbacks by segments for customer
- POST `/api/co-branded-cashback/customer` returned 400 despite active co-branded cashback
- Co-brand. Web API returned empty array of selected groups and merchants
- Co-brand. Mobile API: 404 when retrieving selected customer positions for co-branding
- Mobile API returned incorrect customer cashback % in segment
- Mobile API (`POST /api/customer-cashback/{id}`) did not accept array
- Mobile API incorrectly returned withdrawal value in settings route
- Personal cashback: web gateway returned duplicates of selected merchant/group per customer in analytics
- Personal cashback was saved without uploaded customer file
- Personal cashback accepted invalid customer files
- System rejected segment customer file with valid Customer ID
- Terminal file was not added to cashback with segment
- Terminal file upload in cashback completed with error when using template
- If a merchant has 1 terminal — it was impossible to delete it
- Adding segmentation added new merchant to default cashback
- Incorrect toggle logic in default cashback segmentation
- Default cashback was not saved if merchant's minimum amount was greater than cashback maximum amount
- 500 when deleting merchant in current cashback
- 500 when editing short description with 2000+ characters
- 500 (backend crash) on stage — regression eliminated
- Deeployalty duplicated large number of default cashbacks to next month
- Webhook was not sent to Deeployalty about check NON-matching
- Merchant campaign terminal count from Deeployalty was not duplicated in default cashback
- Error editing merchant when adding invalid terminal values
- Editing operation type name worked incorrectly depending on language localization
- Hook was not sent to Deeployalty about check NON-matching
- Error during user registration via referral code
- Incorrect calculation of registration limit for referral link
- "Last update date" field was not updated after adding customers to personal cashback
- Missing confirmation window when deleting segment customers of merchant
- Upon reaching maximum segment limit, message "Merchant limit reached" was displayed
- Save button for general settings of default cashback did not work
- "Docs" button did not work
- Localization key was displayed in merchant segment form
- Message language did not match interface locale language
- Missing field validation in "Cashback Withdrawal Conditions" subsection; fields allowed negative value entry
- Incorrect error message when withdrawing cashback (same response for different denial scenarios in `/withdraw` route)
- Able to delete Card Product from default cashback of future period without error
- Import file without card type did not work
- `Cannot GET` errors when loading cashback page
- Page scrolled to header when switching pagination
- Layout "floated" depending on number of digits in pagination
- Centered tooltip relative to line in transaction import details
- Missing asterisk of required field next to "Group" field in each cashback type
- "Cashback" item in side menu had incorrect text alignment and displayed unnecessary tooltip on hover
- Lexical error in default cashback settings
- Favicon installed on changelog page
- Business admin granted archiving rights

## [0.6.0] - 2026-03-25

### Added

- Multi-currency support: currency rate reference (NBU), automatic transaction conversion to system currency, saving conversion data (original amount, currency, rate) together with transaction
- Separate `INVALID_TRANSACTIONS` table for invalid transactions (missing currency, unknown rate, etc.) with reason display in user analytics
- New cashback type — co-branded: support for card type settings, parallel operation with default cashback, separate mobile endpoints for retrieving settings and positions
- `Merchant Name` field as additional condition for transaction matching by merchant (together with Terminal ID)
- Additional `terminal_name` and `terminal_location` fields when adding terminals per merchant
- Counter of total loaded terminals for each merchant across all cashback types
- Endpoint for retrieving cashback data for specific transaction (search by transaction ID)
- Paginated endpoint for retrieving full user cashback history (duplicated to mobile gateway)
- `bannerUrl` field in mobile endpoint `/cashbacks/positions` (only for merchant type positions)
- `accountId` field in `CASHBACK_HISTORY` — saves the account to which funds were withdrawn
- Return of `cashbackHistoryId` in response of `/withdraw` endpoint (for reversal operation support)
- [Deeployalty] Sending webhook about user's selected cashback to Deeployalty when changing customer settings
- Deployment of cashback module for "Clearing House" client
- Display of Deeployalty mark on merchants when selecting them during cashback creation
- Ability to view errors by lines when importing transactions
- Display of required fields in all system forms

### Changed

- [Deeployalty] Updated terminal synchronization logic: terminals in "Merchants" section are only supplemented (not deleted), in merchant cashback block — actual terminals of current campaign
- Frontend analytics displays transaction dates in local time (instead of UTC)
- `accountId` field in `/cashback-histories/withdraw` endpoint became required
- Statistics page hidden (data was hardcoded, did not reflect real values)
- Unified pagination on all tabs with table display

### Fixed

- Unable to change Deeployalty merchant settings for next month's cashback
- Cashback was not deducted for refund with currency conversion
- 500 error when withdrawing cashback
- 500 error when withdrawing funds for "Clearing House" client
- Authorization error on Oschadbank server (dev)
- DB connection error (dev Oschadbank)
- Co-branded cashback was accrued without considering card type
- Active co-branded cashback for current month was available for editing
- Unable to disable currency conversion during initial system setup
- "Save" button was active even without changes in currency API settings
- UAH transaction only passed in uppercase (UPPERCASE)
- Oracle: `IN()` limitation of 1000 elements — fixed with batch processing
- After logging out, redirect went to `/setup-database` instead of `/login`
- Missing redirect to login page after confirming logout in Settings section
- Column names `CreatedAt`, `Parent ID`, `LDAP Filter` were not translated to Ukrainian in respective sections
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
- MD-format support in "Description" field by merchant and value delivery to API
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
- [Deeployalty] Retrieval of max cashback amount per merchant field during Deeployalty integration
- [Deeployalty] Cron job run once per hour for cashback offer updates
- Removal of CurrentCashbackResponse and related logic
- Refactoring of cashback service logic for personal/default cashbacks
- Refactoring of refund transaction processing

### Fixed
- Cron job overlap
- Getting cashbacks by certain type did not work
- Transaction import available to non-admin
- Incorrect Deeployalty production link for cashback integration
- HTTPS dev stage error
- Production council: merchants marked as from Deeployalty
- Referral program available to non-admin
- Production council: previously added users missing in configurator
- Production council: GET error when navigating to webhooks section
- Unable to export customer list for personal cashback
- Transaction import — floating message not translated when changing interface language
- Errors when working with WebPlatform API clearing house
- Blocked merchant settings fields when adding Deeployalty merchant to default/personal cashback
- Error adding merchant to current default cashback if it contained Deeployalty merchant
- Incorrectly accrued cashback per merchant
- Creating personal cashback
- Deeployalty time zone

## [0.4.0] - 2026-02-12

### Added
- CRUD for cashback operation types in admin panel
- Automatic initialization of nv-keys in Vault on first run
- Import and export of terminal list for cashback per merchant
- Transaction upload section in admin panel
- Settings section in admin panel with ability to:
  - update LDAP connection and role mappers
  - manage manually created users
  - change access for editing past cashback periods
  - configure webhooks
  - add new access keys for Mobile Gateway (authorization)
  - change database settings (not recommended after initial setup)
  - add new data sources (for example, transaction download from Kafka)
- New endpoint `GET /api/v2/cashback/current` for retrieving current cashback settings, positions and selected card products in one request

### Changed
- Webhook mapping routes with field relocation to subscriptions
- Validation of "Description" field in merchants: all special characters allowed
- Removal of deprecated endpoints:
  - `GET /customer-cashback/chosen/:id`
  - `GET /settings`
  - `GET /allowed-cashbacks/positions`
  - `GET /receipt/transaction/:id`
- Personal cashback now completely overrides default (previously allowed positions were merged)
- `accountId` field in `POST /cashback-histories/withdraw` became optional (previously required)

### Fixed
- 500 when adding personal cashbacks via backend
- Incorrect logout and missing redirect to authorization page
- Display of % payment fields in default cashbacks with validation errors
- LDAP: missing group in Keycloak and unavailable connection check when updating
- Missing transactions in user analytics and transaction import errors
- Terminal import error
- Incorrect endDate when creating cashback

## [0.3.0] - 2026-02-03

### Added
- Vault integration with environments, services, secret import and documentation
- Deeployalty integration - downloading merchants, terminals and cashback programs
- Kafka library, connection manager and consumer management
- DataSources and DataSourceMappers modules with CRUD, test endpoints and schema analyzer
- Full Webhooks stack: subscriptions, mappers, events, delivery logs and payload templates
- Keycloak configuration module with LDAP endpoints, groups/roles and statistics
- MinIO module and bucket/policy initialization scripts
- Mobile access keys module and endpoints

### Changed
- CI/CD pipelines and GitHub Actions for building/publishing images with version tags
- Dockerfile and docker-compose for local and production environments, nginx/filebeat configs
- Dynamic database: TS seeds, new migrations, bulk-json-save and SQL helpers
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
    - `WEBHOOK_MAPPERS`: removed `AUTHORIZATION_TYPE`, `HTTP_METHOD` columns (moved to `WEBHOOK_SUBSCRIPTIONS`)
    - `WEBHOOK_EVENTS`: `DESCRIPTION` → `DESCRIPTION_EN` + added `NAME_EN`, `NAME_UA`, `DESCRIPTION_UA` (multilang)
  - Migrations:
    - many old migrations removed/consolidated, new point migrations added for changes above
- Logging and error handling in key services (centralized handlers)
- Transaction and cashback processing: SQL operations, batch/parallel, flexible dates
- Keycloak/Redis/Deeployalty configurations and vault paths structure for environments

### Fixed
- Reliability of initialization and logging during connections to DB/Redis/Kafka

## [0.2.0] - 2025-12-03

### Added
- Logic for loading and processing files with customer ID lists for personal cashbacks
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
- Validation for group, MCC, card product, and merchant creation/editing forms
- Additional attempts to search transaction for check matching
- Restrictions on adding files with terminal ID
- Reversal of accruals on add-cashback and withdraw errors

### Changed
- "Program Name" field for referral promotions is now required when creating promotional link
- Minimum amount per merchant is now required field for personal cashbacks
- In user analytics, cashback and referral program data displayed in separate menus
- Hidden unavailable dates when creating cashback
- Hidden list of terminals per merchant
- [DevOps] Limited log storage in Elasticsearch (Kibana) to 3 months
- Fixed handling of "Current customer balance less than minimum withdrawal amount" cases

### Fixed
- Customer can no longer register using their own referral code
- Fixed 500 error when registering new user via referral code
- Fixed "Registration limit reached" error when changing maximum user registrations
- Fixed "Back" button name and section for user program
- Edit promotional link form no longer closes when copying code
- Fixed "Registration limit reached" error when registering via promotional code
- Fixed display of table with link data
- Fixed display of invited/inviter amount in user analytics
- Blocked registration via inactive by date promotional link
- Fixed displaying data table by links when re-clicking section
- Fixed exceeding refund
- Fixed dataset when exporting customer file for personal cashback