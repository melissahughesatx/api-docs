
# Manage the payables workflow 

A payable is defined as any financial document that is given by a supplier to an entity. A payable itemizes and demands payment for goods or services. A payable may come in the form of a:

- Bill
- Incoming invoice
- Account payable document

Monite provides flexible payables management for each *entity* and the *end users* who work for that entity. You easily control access to payables based on the role assigned to each end user. This flexibility allows partners to manage workflows easily through the user hierarchy.

This page explains the payables workflow and shows you how to implement it.

## How it works

Each payable in Monite goes through the following lifecycle:

![](../assets/images/payable-states.png)

With a few API calls you enable entity and entity users to upload, view and edit payables. Users with a different role follow the approvals workflow until a payable is rejected or paid.

## What you need

In order to follow the workflow in this page you need to:

* Complete the [Get started](b.get-started.md)


## Implement the payables workflow

This section shows you the API calls you need to implement in your UI for your entity and entity users to manage their accounts. 

### 1. Upload payables

Entities and entity users easily upload any kind of payable. When the payable is uploaded for clearance and payment, the Monite Optical Character Recognition (OCR) reader processes the details from the uploaded documents. Monite maps the information from the payable to the correct field and sends this information back to the user in the return parameters of the API call.

To upload a payable: 

1. Use the partner-level access token retrieve an entity access token with a call to `GET /partner api/api_users/v1/entities/{entity_id}/access`:
    ```curl
    curl --location --request POST 'https://api.dev.monite.dev/partner-api/api_users/v1/entities/au-your0000-3nt-1tty-idlkj-mycompany/access' \
      --header 'x-api-key: au-your0000-part-ner0-leve-access0token'
    ```
    The successful response code 200 OK contains the entity token:
    ```json
    {
        "token": "en-your0000-enti-ty00-leve-access0token",
        "expiration_in": "2025-11-25T05:30:06.804869+00:00"
    }
    ```

2. Submit the payable with a call to:
   - For an entity:   `POST /partner-api/entities/v1/payables`
   - For an entity user: `POST /partner-api/entities_users/v1/payables`
    ```curl
    curl --location --request POST 'https://api.dev.monite.dev/partner-api/entities/v1/payables' \
    --header 'Content-Type: multipart/form-data' \
    --header 'x-api-key: en-your0000-enti-ty00-leve-access0token' \
    --form 'file=@"BillExample.jpg"'
    ```
    This call submits and analyses the payable uploaded. For example, the return parameters when you upload the [bill example](.../assets/images/BillExample.jpg) are:
    
[stuff](/assets/images/BillExample.jpg)
[stuffs](../assets/images/BillExample.jpg)
[stuffs](./assets/images/BillExample.jpg)


    
    ```json
    {
        "id": "aa314fdd-a763-4920-a8c8-6285fc1745c0",
        "entity_id": "en-your0000-enti-ty00-1D3799b65bcf",
        "status": "new",
        "source_of_payable_data": "ocr",
        "currency": "EUR",
        "amount": 11900,
        "description": null,
        "due_date": null,
        "issued_at": "2021-11-11",
        "counterpart_bank_id": null,
        "counterpart_account_id": null,
        "counterpart_name": "SupplierName",
        "payable_origin": "upload",
        "was_created_by_external_user_name": null,
        "was_created_by_external_user_id": null,
        "created_at": "2021-11-25T05:10:35.211432+00:00",
        "updated_at": "2021-11-25T05:10:35.211447+00:00",
        "file": {
            "file_type": "ocr-files",
            "name": "file",
            "region": "eu-central-1",
            "md5": "027736d1a331d7a1d4085051ed07ee3e",
            "mimetype": "image/jpeg",
            "url": "https://monite-ocr-files-eu-central-1-develop.s3.amazonaws.com/4a8c2058-1c5b-449d-96b5-bf117ec864bb/f8679381-a929-461e-b4a2-ceed059e8ab2",
            "size": 246331,
            "previews": []
        }
    }
    ````

For more information about these fields, see the [payable schema](c2NoOjI3NzgxOTUx-payable-response-schema).


### 2. View all payables

When entity and entity users have uploaded some payables, they can easily view all the payables in their Monite space. To allow the right level of access to the authorized users, Monite allows certain entity users to view the but not change uploads.

For example, a member from the Finance team in an entity wants to view all the uploads for record keeping. However, this person has restricted access. 

To view all payables, call:

- entity: `GET /partner-api/entities/v1/payables`
- entity user:`GET /partner-api/entities_users/v1/payables`
```curl
curl --location --request GET 'https://api.dev.monite.dev/partner-api/entities/v1/payables' \
  --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
```

Monite returns a paged series of all payables the entity has access to. For example: 
```json
{
    "data": [
        {
            "id": "aa314fdd-a763-4920-a8c8-6285fc1745c0",
            "entity_id": "b0ff50d0-cdea-42fd-9461-1b3799b65bcf",
            "status": "new",
            "source_of_payable_data": "ocr",
            "currency": "EUR",
            "amount": 11900,
            "description": null,
            "due_date": null,
            "issued_at": "2021-11-11",
            "counterpart_bank_id": null,
            "counterpart_account_id": null,
            "counterpart_name": "SupplierName",
            "payable_origin": "upload",
            "was_created_by_external_user_name": null,
            "was_created_by_external_user_id": null,
            "created_at": "2021-11-25T05:10:35.211432+00:00",
            "updated_at": "2021-11-25T05:10:35.211447+00:00"
        }
    ],
    "prev_page": null,
    "next_page": "/partner-api/entities/v1/payables?pagination_token=Zmlyc3Rfb2lkPTEmbmV4dF90b2tlbj0y"
}
```
`data` is an array of uploaded payables. `prev_page` and `next_page` are [pagination tokens](y.filtering-sorting-pagination.md).

### 3. Edit a payable

Sometimes information submitted in a payable needs to be updated. Once a payable is uploaded, entity and entity users can easily update the information extracted from the payable and make changes before the payable is submitted. Monite also enables entities and entity users to edit any section information. 

To update a payable:

1. Retrieve the payable from Monite:
   - entity: `GET /partner-api/entities/v1/payables/{payable_id}`
   - entity user:`GET /partner-api/entities_users/v1/{payable_id}`

   ```curl
    curl --location --request GET 'https://api.dev.monite.dev/partner-api/entities/v1/payables/<Payable Id>' \
    --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
    The successful response code 200 OK contains the [payable attributes](c2NoOjI3NzgxOTUx-payable-response-schema)

2. In your UI, enable users to update this payable.
3. Upload changes to the payable :
   - Entity: `PUT /partner-api/entities/v1/payables/{payable_id}`
   - Entity user: `PUT /partner-api/entities_user/v1/payables/{payable_id}`

    For example, to update Bill amount and Due Date:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/<Payable Id>' \
      --header 'Content-Type: application/json' \
      --header 'x-api-key: en-your0000-enti-ty00-leve-access0token' \
      --data-raw '{
        "amount": 238,
        "due_date": "2021-12-12"
    }'
    ```

### 4. Approve a payable

After uploading, payables undergo the following approval stages before clearing for payment:
- `approval_in_progress`
- `waiting_to_be_paid` 

Depending on [payable status](#how-it-works) you need to implement the following stages:

- **Cancel a payable**: when the end users want to stop an upload, you cancel it with a call to `PUT /partner-api/entities/v1/payables/{payable_id}/cancel`:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/aa314fdd-a763-4920-a8c8-6285fc1745c0/cancel' \
      --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
    The `200 OK` response contains the following status:

     `"status": "cancelled"`

- **Start approval**: Once the payable has been uploaded and recognized, to stat the approval process, call `PUT /partner-api/entities/v1/payables/{payable_id}/assign_to_payment`:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/aa314fdd-a763-4920-a8c8-6285fc1745c0/start_approve' \
      --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
    The `200 OK` response contains the following status:

    `"status": "approve_in_progress"`

- **Confirm payment**: To approve the payable, call `PUT /partner-api/entities/v1/payables/{payable_id}/approve_payment_operation`:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/aa314fdd-a763-4920-a8c8-6285fc1745c0/approve_payment_operation' \
      --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
  The `200 OK` response contains the following status:

    `"status": "waiting_to_be_paid"`

- **Reject a payable**: If the approver finds any mismatch or discrepancies in the payable, they decline with a call to `PUT /partner-api/entities/v1/payables/{payable_id}/reject`:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/aa314fdd-a763-4920-a8c8-6285fc1745c0/reject' \
    --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
   The `200 OK` response contains the following status:

    `"status": "rejected"`

- **Pay**: once a payable is approved and assigned for payment, you change the status to `paid` with a call to `PUT /partner-api/entities/v1/payables/{payable_id}/pay`:
    ```curl
    curl --location --request PUT 'https://api.dev.monite.dev/partner-api/entities/v1/payables/aa314fdd-a763-4920-a8c8-6285fc1745c0/pay' \
      --header 'x-api-key: en-your0000-enti-ty00-leve-access0token'
    ```
    Successful response 200 OK contains updated paybable's status: `"status": "paid"`. 



## Reference

For more information about the workflow explained in this page, see the:

- [Payable schema](c2NoOjI3NzgxOTUx-payable-response-schema)
- [Pagination tokens](y.filterings-sorting-pagination.md)
- [Payable attributes](c2NoOjI3NzgxOTUx-payable-response-schema)

Currently, partial payments are not supported.