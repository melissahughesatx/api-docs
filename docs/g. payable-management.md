## What is a Payable

Payable is defined as any financial document that is given by an entity's supplier itemizing the purchase of a good or a service and demanding payment. A 'Payable' could in the form of either:

- Bill
- Incoming invoice
- Account Payable document

Monite's platform provides the flexibility both for the *entity* and the *end users of an entity* to manage a payable. However, based on the type of user accessing the respective APIs, there might be certain restrictions which are imposed and controlled by the permissions. This type of flexibility allows the partner to create a funnel/hierarchy of the users.

### 1. Upload a Payable

The **Monite Platform** provides the capability to their users to upload any kind of payable, such as a bill, an invoice or an accounts payable document as described above.

**How to upload as an entity**

Using the access token generated for the entity via the `/partner api/api_users/v1/entities/{entity_id}/access` API, the entity can then submit a payable by using the `POST /partner-api/entities/v1/payables`. This API successfully submits a payable for an entity. 

**How to upload as an end user of an entity**

A payable - as described above can also be uploaded for clearance & payment, by the end user of an entity. For example, an employee submitting their communication expenses for a payment. Using the `POST /partner-api/entities_users/v1/payables` API, the entity allows any of it's users to submit such an expense

Upon a successful upload of a payable, the in built OCR reader starts processing and reads the details from the upload. Once the OCR processing is finished, it maps the right information from the payable to the exact column and displays the final result to the user(s). 

To provide additional control over the uploaded payable, our API platform provides the capability to the user to edit any pre-filled information from OCR and make the final changes before submission

### 2. Edit an Uploaded Payable

While a payable has been successfully submitted, there are situations where the submitted information may need to be added/edited. Our platform APIs provide such an option for both our entity end users and the entity itself

**How to edit an upload as an entity**

As the entity is the final responsible authority for clearing a payment against the upload, it is of utmost importance to provide the entity with the correct set of tools for the editing them. Using the `PUT/partner-api/entities/v1/payables/{bill_id}` API allows the entity to edit any information to an already uploaded payable. 

**How to edit an upload as an end user of the entity**

An uploaded payable may need an edit by the end user based on any new information which is available and needs to be submitted. The `PUT/partner-api/entities_user/v1/payables/{bill_id}` API provides the capability to the end user to edit such an information. This API also allows editing any section information which has been asked by the finance admin to be provided by the end user

### 3. View all Payables

Once the payables have been successfully uploaded either by the entity or the end users of an entity, the Monite's platform provides the option to view all the uploaded payables

**How to view all the uploads as an entity**

For any kind of financial management of the expenses by the entity, the `GET/partner-api/entities/v1/payables` API provides the capability to access all the payables uploaded, with the parent access

**How to view all the uploads as a user in the entity**

To allow the right level of access to the authorized users, our platform allows certain type of entity users to view the uploads without allowing the unlimited control over the payable. 

A member from the Finance team of an entity wants to view all the uploads for record keeping. However to prevent the misuse, the finance team member has restricted access. Using the `GET /partner-api/entities_users/v1/payables` API in conjunction with the generated user token displays the results to the finance user based on their assigned restrictions.

The generated access token with the type of API called helps our system identify the user and the type of permissions assigned to such a user. This allows us to display the appropriate amount of information.

### 4. Managing all Payables

Upon successfully uploading a payable, the next step involves evaluation, assigning and management of these. With the robustness of our system, we provide a full set of capability for managing these

**Delete an Upload**

Using either the `DELETE/partner-api/entities/v1/payables/{payable_id}` API, the entity can delete any specific upload. This marks the bill as archieved in the system.

The `DELETE/partner-api/entities_users/v1/payables/{payable_id}` API allows the users i.e. the financial team members to remove or delete a bill from the system

**Stages after the Upload**

Since the bill after uploading undergoes various approval stages before finally clearing for payment, our platform provides the capabilities to manage the states of an upload using our simple to use API

- *Assign for payment*: Once the bill has been uploaded and accepted, the bill will then be assigned for payment using the `PUT/partner-api/entities/v1/payables/{payable_id}/assign_to_payment`API
- *Process for paying*: As soon as the bill/upload has been approved and assigned for payment, while the payment is being made and the bill is being settled, the `PUT/partner-api/entities/v1/payables/{payable_id}/pay` API moves the bill to the processing stage providing the exact visibility to the entity users.
- *Confirm a payment*: Upon successfully paying an upload, the bill would be moved to the status of confirming the payment. The `PUT/partner-api/entities/v1/payables/{payable_id}/confirm_payment_operation`API when used moves confirms the payment against the bill
- *Decline a payable*: If the approving authority finds any mismatch or discrepancies in the upload, they can simply decline the bill by using `PUT/partner-api/entities/v1/payables/{payable_id}/decline`API from their system.
