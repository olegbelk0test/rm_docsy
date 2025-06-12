---
tags: [purchase-page, developers, sdk, api]
sidebar_position: 70
sidebar_label: Public API
sidebar_class_name: green
---

> **Note** Names and internal tool references have been anonymised for portfolio use.

# Purchase Pages Public API

In this article, you will find the Purchase Pages API Reference Guide.

---

## IPurchasePagesService

`IPurchasePagesService` is the service for getting a batch of purchase pages:    

<details>
<summary>IPurchasePagesService</summary>

```csharp
public interface IPurchasePagesService
{
    /// <summary>
    /// Gets purchase pages by pageTypeIds.
    /// </summary>
    /// <param name="pageTypeIds">IDs of the page types to resolve</param>
    /// <param name="token">CancellationToken</param>
    /// <exception cref="OperationCanceledException">
    /// Thrown if the cancellation token is cancelled
    /// </exception>
    /// <exception cref="PurchasePagesException">
    /// Thrown on error – see <see cref="PurchasePagesExceptionType"/>
    /// </exception>
    UniTask<PurchasePage[]> GetPurchasePagesByPageTypeIdsAsync(
        IEnumerable<int> pageTypeIds,
        CancellationToken token = default);
}
```
</details>

### PurchasePage

A **PurchasePage** represents user purchase page general information:

<details>
    <summary>PurchasePage</summary>

```csharp
public class PurchasePage
{
    /// <summary>Unique identifier of the page configuration</summary>
    public int PageId { get; set; }

    /// <summary>
    /// Array of denomination lines (purchase options)
    /// </summary>
    public PageOption[] Options { get; set; }

    /// <summary>
    /// Identifier of the page‑type group this page belongs to
    /// </summary>
    public int PageTypeId { get; set; }

    /// <summary>Date/time when the page becomes active</summary>
    public DateTimeOffset? StartDate { get; set; }

    /// <summary>Date/time when the page becomes inactive</summary>
    public DateTimeOffset? EndDate { get; set; }

    /// <summary>
    /// Identifier of the page resolved for the current user at request time
    /// </summary>
    public string ResolveRequestId { get; set; }

    /// <summary>
    /// Studio‑defined JSON block (configured in the back‑office tool); may include art URLs
    /// </summary>
    public PurchasePageAdditionalData AdditionalData { get; set; }
}
```
</details>

### PurchasePagesExceptionType

PurchasePagesExceptionType enumerates the high‑level error categories returned by the service:

<details>
    <summary>PurchasePagesExceptionType</summary>

```csharp
public enum PurchasePagesExceptionType
{
    /// <summary>Unexpected error</summary>
    GenericException,

    /// <summary>Failure to send an authorised request</summary>
    IdentityException,

    /// <summary>No Internet connection detected</summary>
    NoInternet
}
```
</details>
