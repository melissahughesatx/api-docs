
# Manage the payables workflow 

A payable is defined as any financial document that is given by a supplier to an entity. A payable itemizes and demands payment for goods or services. A payable may come in the form of a:

- Bill
- Incoming invoice
- Account payable document

Monite provides flexible payables management for each *entity* and the *end users* who work for that entity. You easily control access to payables based on the role assigned to each end user. This flexibility allows partners to manage workflows easily through the user hierarchy.

This page explains the payables workflow and shows you how to implement it.

## How it works

The following figure shows the payables workflow:

TBD.



## What you need

In order to follow the workflow in this page you need to:

* Complete the [Get started](b.get-started.md)
* TBD \<Something to do API calls with> 

## Implement the payables workflow


TBD: what we really need is a postman collection or such like with this workflow. 

### 1. Upload payables

Monite provides the capability for entities and entity users to easily upload any kind of payable. When the payable is uploaded for clearance and payment, the Monite Optical Character Recognition (OCR) reader processes the details from the uploaded documents. Monite maps the information from the payable to the correct field and sends this information back to the user in the return parameters of the API call.

TBD: is this information storied in a database at the same time?

To upload a payable: 

1. Retrieve an access token for the entity with a call to:

   `GET /partner api/api_users/v1/entities/{entity_id}/access`

2. Submit the payable with a call to:
   - For an entity:   `POST /partner-api/entities/v1/payables`
   - For an entity user: `POST /partner-api/entities_users/v1/payables`

   This call submits and analyses the payable uploaded. For example, the return parameters when you upload the [bill example](../assets/images/BillExample.jpg) are:

```json
{
    "id": "c38552fe-3a20-47cf-baff-646d7107cfd8",
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
    "created_at": "2021-11-17T15:05:54.631508+00:00",
    "updated_at": "2021-11-17T15:05:54.631524+00:00",
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

For more information about these fields, see [payable schema](c2NoOjI3NzgxOTUx-payable-response-schema).


### 2. Edit a payable

There are situations where information submitted in a payable needs to be added to or edited. Once a payable is uploaded, entity and entity users can easily update the information extracted from the payable and make changes before the payable in Monite is submitted.  Monite also enables entities and entity users too edit any section information which has been asked by the finance admin. 

To edit an update a payable:

1. Retrieve the payable from Monite:

   TBD: which API do we call here?

3. Update information in the payable downloaded from Monite.
4. Publish changes to the payable:
    - Entity: `PUT /partner-api/entities/v1/payables/{bill_id}`
    - Entity user: `PUT /partner-api/entities_user/v1/payables/{bill_id}`
 


### 3. View all payables

When entity and entity users have uploaded some payables, they can easily view all payables so far uploaded. To allow the right level of access to the authorized users, Monite allows certain type of entity users to view the uploads without allowing the unlimited control over the payable.

For example, a member from the Finance team of an entity wants to view all the uploads for record keeping. However to prevent the misuse, the finance team member has restricted access. 

To view all payables, call:

- entity: `GET /partner-api/entities/v1/payables`
- entity user:`GET /partner-api/entities_users/v1/payables`

Monite returns a paged series of all payables the entity has access to. For example: 
```json
{
    "data": [
        {
            "id": "f53f66ee-8ef8-4319-b70b-fcd8e0a6e07f",
            "entity_id": "b0ff50d0-cdea-42fd-9461-1b3799b65bcf",
            "status": "unpaid",
            "source_of_payable_data": "ocr",
            "currency": "EUR",
            "amount": 11900,
            "description": null,
            "due_date": null,
            "issued_at": "2021-11-11",
            "counterpart_bank_id": null,
            "counterpart_account_id": null,
            "counterpart_name": "B'H From SupplierName",
            "payable_origin": "upload",
            "was_created_by_external_user_name": null,
            "was_created_by_external_user_id": null,
            "created_at": "2021-11-11T13:56:07.599476+00:00",
            "updated_at": "2021-11-11T13:56:07.599504+00:00"
        }
    ],
    "prev_page": null,
    "next_page": "/partner-api/entities/v1/payables?pagination_token=Zmlyc3Rfb2lkPTEmbmV4dF90b2tlbj0y"
}
```
where data is an array of uploaded paybles and prev_page and next_page are [pagination tokens](y.filterings-sorting-pagination.md).

### 4. Manage payables

Upon successfully uploading a payable, the next step involves evaluation, assigning and management of these. With the robustness of our system, we provide a full set of capability for managing these

After uploading, payables undergo various approval stages before finally clearing for payment, the approval stages for a payment are: 

![](../assets/images/payable-states.png)

- *Delete an Upload* - monite enables financial team members to remove or delete a bill from the system. The entity can delete any specific upload. This marks the bill as archived in the system.

   - entity : `DELETE /partner-api/entities/v1/payables/{payable_id}`,
   - entity user: `DELETE/partner-api/entities_users/v1/payables/{payable_id}`

- *Cancel Payable*: when the end users want to stop an upload, cancel by using `PUT /partner-api/entities/v1/payables/{payable_id}/cancel`API from their system.
- *Assign To Payment*: Once the bill has been uploaded and recognized, its approval for a payments starts with the `PUT /partner-api/entities/v1/payables/{payable_id}/assign_to_payment`API
- *Confirm Payment*: To finally approve the bill for payment call the `PUT /partner-api/entities/v1/payables/{payable_id}/confirm_payment_operation` API
- *Reject Payable*: If the approving authority finds any mismatch or discrepancies in the upload, they can simply decline the bill by using the `PUT /partner-api/entities/v1/payables/{payable_id}/decline` API from their system.
- *Pay*: As soon as the bill/upload has been approved and assigned for payment, you can change its status to "paid" by calling the `PUT /partner-api/entities/v1/payables/{payable_id}/pay` API.

## Reference

For more information about the workflow explained in this page:

- Something
- Something else