# Changelog

All notable changes to this project are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Remote methods for additional tax identifiers: `createAdditionalTaxIdentifier`, `getAdditionalTaxIdentifier`, `updateAdditionalTaxIdentifier` and `deleteAdditionalTaxIdentifier`.
- Remote methods for C5 activation, including the IRAS email and redirect flows: `createC5Activation`, `createC5Deactivation`, `activateC5Email`, `deactivateC5Email`, `cancelC5Email`, `activateC5Redirect`, `deactivateC5Redirect` and `cancelC5Redirect`.
- `discoverExists` and `discoverIdentifiers`, for checking whether a participant exists on the network and listing the identifiers used in each country.
- `getDocumentSubmissionEvidence`, `updatePeppolIdentifier`, `createReceivedDocument` and `getReceivedDocument`.
- The records these methods use, such as `AdditionalTaxIdentifier`, `C5ActivationRequest`, `CountrySpecifications`, `DocumentSubmissionEvidence`, `ReceivedDocument` and `ReceivedDocumentCreate`, and new document types on `SendableDocument` (`order`, `invoiceReport`, `frEReport`, `paymentReceivedNotification` and `envelopedData`).
- `GetPurchaseInvoiceQueries`, `GetPurchaseInvoicePackagedQueries` and `GetReceivedDocumentQueries`, which carry the query parameters of those methods, including the new `version` parameter.
- An optional `headers` parameter on every remote method, for sending extra request headers.
- New fields on existing records, for example on `Invoice`, `InvoiceLine`, `LegalEntityCreate`, `LegalEntityUpdate`, `Party`, `PaymentMeans` and `Routing`.
- New `ConnectionConfig` fields, taken from `http:ClientConfiguration`: `followRedirects`, `cookieConfig`, `socketConfig` and `laxDataBinding`.

### Changed

- **Breaking:** these remote methods are renamed:

    | 1.5.2 | 2.0.0 |
    |---|---|
    | `discoveryReceives` | `discoverReceives` |
    | `showInvoiceSubmissionEvidence` | `getInvoiceSubmissionEvidence` |
    | `getInvoiceJson` | `getPurchaseInvoice` |
    | `getInvoiceUbl` | `getPurchaseInvoicePackaged` |
    | `getInvoiceUblVersioned` | `getPurchaseInvoiceVersioned` |
    | `getWebhookInstances` | `getWebhookInstance` |

- **Breaking:** the `pmv` query parameter of `getPurchaseInvoice` is no longer positional. Pass it as a named argument (or as a `GetPurchaseInvoiceQueries` record):

    ```ballerina
    // 1.5.2
    storecove:PurchaseInvoice invoice = check storecove->getInvoiceJson(guid, "2.0");
    ```

    ```ballerina
    // 2.0.0
    storecove:PurchaseInvoice invoice = check storecove->getPurchaseInvoice(guid, pmv = "2.0");
    ```

- **Breaking:** the `packaging` and `packageVersion` parameters of `getPurchaseInvoicePackaged` and `getPurchaseInvoiceVersioned` are string-literal unions of the accepted values (was `string`).
- **Breaking:** `deleteLegalEntity`, `deleteAdministration`, `deletePeppolIdentifier` and `deleteWebhookInstance` return `error?` (was `http:Response|error`).
- **Breaking:** record fields use camel case, mapped to the API's snake-case JSON names with `@jsondata:Name`. For example, `Administration.legal_entity_id` is now `legalEntityId`, `LegalEntityCreate.party_name` is now `partyName`, and `InvoiceLine.taxes_duties_fees` is now `taxesDutiesFees`.
- **Breaking:** these records are renamed:

    | 1.5.2 | 2.0.0 |
    |---|---|
    | `EIdentifiers` | `ElectronicIdentifiers` |
    | `PaymentMeansDeprecated` | `PaymentMeansDEPRECATED` |
    | `PurchaseinvoiceDelivery` | `PurchaseInvoiceDelivery` |
    | `PurchaseinvoiceParty` | `PurchaseInvoiceParty` |
    | `ThePaymentTerms` | `PaymentTerms` |
    | `VatDetails` | `VATDetails` |

- **Breaking:** `AccountingSupplierParty.party` is a `Party` (was the connector's own `SupplierParty`).
- **Breaking:** `Delivery.actualDate` and `Delivery.location` are replaced by `actualDeliveryDate` and `deliveryLocation` (a `DeliveryDeliveryLocation`).
- **Breaking:** many fields that were `string` are typed as string-literal unions of the values the API accepts, for example `Country`, `CurrencyCode`, `Invoice.invoiceType`, `PaymentMeans.code`, `Tax.category` and `InvoiceLine.quantityUnitCode`. Date fields such as `Invoice.issueDate` carry a pattern constraint.
- **Breaking:** `PurchaseInvoiceTaxElement.amount` is a `decimal` (was `string`).
- **Breaking:** `ConnectionConfig.http1Settings` is now `http:ClientHttp1Settings` (was the connector's own `ClientHttp1Settings`).
- `ConnectionConfig.timeout` defaults to 30 seconds (was 60).

### Removed

- **Breaking:** the `SupplierParty`, `DeliveryLocation` and `ErrorModel` records.
- **Breaking:** fields that the API no longer defines: `AccountingCustomerParty.accountId`, `InvoiceLine.delivery`, `RawDocumentData.documentTypeId` and `RawDocumentData.processId`, `PurchaseInvoiceInvoiceLine.price_amount` and `quantity`, and `PurchaseInvoice.amount_including_tax`, `payable_amount`, `payable_rounding_amount` and `prepaid_amount`.
- **Breaking:** the connector's own `ClientHttp1Settings` and `ProxyConfig` records. Use `http:ClientHttp1Settings` and `http:ProxyConfig` instead.
