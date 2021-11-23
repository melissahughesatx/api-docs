
# Get started with Monite API

Monite offers the following roles:

- Partner - a person who implements Monite in their SASS tool
- Entity - the customer of a partner
- End user - the person who uses the software products that access Monite

All calls to Monite are totally secure, they require an access token to succeed. The information in the token tells Monite the role assigned to the person making the API call. After a Partner contacts Monite, they retrieve access tokens and use the API to set up a space for each entity in Monite.

This section shows you how to create the [authentication tokens](d.authentication.md) and entities that are the first step for all development solutions. 

## How it works

The following figure shows the API callflow for a partner to set up an entity, and for an end user to start managing expenses inside Monite for that entity. 

![](../assets/images/monite-workflow.png)

## What you need

To successfully understand and complete this task you must have the following:

- [A valid Monite account](v.manage-your-monite-account.md#setup-a-monite-account)
- [A Monite API key](v.manage-your-monite-account.md#retrieve-your-monite-credentials)

## Implement your first Monite app

In order to create your first Monite app you need to:


1. **Check your authentication status**

   Call `GET 'https://api.dev.monite.dev/partner-api/api_users/v1/auth'` with the API `user_id` and `secret` supplied with your Monite account.

    ```curl
   curl --location --request GET 'https://api.dev.monite.dev/partner-api/api_users/v1/auth' \
     --header 'Content-Type: application/json' \
     --data-raw '{
       "user_id": "7bf61815-ad2a-42e9-b966-c1e2ac15cd7b",
       "secret": "12345678"
     }'
    ```

    For an authenticated account, Monite returns `200 OK` and you continue this workflow. 

3. **Retrieve a new API token**

    Call `POST https://api.dev.monite.dev/partner-api/api_users/v1/auth` with the API `user_id` and `secret` supplied with your Monite account. 
    ````curl
   curl --location --request POST 'https://api.dev.monite.dev/partner-api/api_users/v1/auth' \
   --header 'Content-Type: application/json' \
   --data-raw '{
   "user_id": "<Your Monite ID>",
   "secret": "<Your Monite secret>"
   }'
    ````
   The return parameters contain a token that gives you admin access to all `/partner-api/api_users/` resources. You use these endpoints to configure your entities. That is, your customers.
    ````json
   {
     "data": {
     "token": "a-m0nit3-a0th-4f81-ad5f-t0k3n",
     "type": "api-key"
     }
   }
   ````


3. **Add an entity to Monite**

    An entity is the customer of a Monite partner. An entity represents a legal entity, that is a company or an individual. To create a new entity for a partner, use the partner token you retrieved previously to call `POST https://api.dev.monite.dev/partner-api/api_users/v1/entities`:

    ```curl
   curl --location --request POST 'https://api.dev.monite.dev/partner-api/api_users/v1/entities' \
     --header 'x-api-key: a-m0nit3-a0th-4f81-ad5f-t0k3n' \
     --header 'Content-Type: application/json' \
     --data-raw '{
     "name": "Castro GMBH",
     "type": "business",
     "country_code": "DE"
   }' 
    ```
   The return parameters for a `200 OK` response contain the information for this entity:
   ```json
   {
     "id": "a-m0nit3--3ntt-1tty-1dntity",
     "name": "Castro GMBH",
     "type": "business",
     "country_code": "DE",
     "internal_id": null,
     "status": "active",
     "created_at": "2021-11-23T09:21:55.551822+00:00",
     "updated_at": "2021-11-23T09:21:55.551837+00:00",
     "mailboxes": [
     {
       "id": 13,
       "entity_id": "a-m0nit3--3ntt-1tty-1dntity",
       "status": "active",
       "related_object_type": "payable",
       "mailbox_name": "302374812897993791_payables",
       "mailbox_full_address": "302374812897993791_payables@dev.monite.dev",
       "belongs_to_mailbox_domain_id": null
     }
     ]
   }
   ```

4. **Grant the entity access to Monite**

   The space inside Monite dedicated to an entity is secured and accessible by that entity only. Each space is secured using entity level tokens. Each entity has its own token. You use the entity ID you retrieved previously and your partner token to create the entity token:

   ```curl
   curl --location --request POST 'https://api.dev.monite.dev/partner-api/api_users/v1/entities/a-m0nit3--3ntt-1tty-1dntity/access' \
     --header 'x-api-key: a-m0nit3-a0th-4f81-ad5f-t0k3n'
   ```

   The return parameters for a `200 OK` response contain the entity token:
   ```json
   {
   "token": "a-m0nit3-a0th--t0kn-fora-3nt1ty",
   "expiration_in": "2021-11-23T10:32:20.719542+00:00"
   }
   ```

5. **Start work**

     You have created the simplest of entities. This entity uses their authentication token to get to work. For example, to upload the first payable:   

   ```curl
   curl --location --request POST 'https://api.dev.monite.dev/partner-api/entities/v1/payables' \
     --header 'x-api-key: a-m0nit3-a0th--t0kn-fora-3nt1ty' \
     --header 'Content-Type: text/plain' \
     --data-binary '@'
   ```
   `data-binary` contains a payable. Monite reads the invoice and adds the data into the space for this entity. The entity user can then update, delete or even act on the payable.  

Would you like something more complicated? Sorry, it's that easy at Monite. A few API calls and your customers are ready to work. 

## Test the workflow

Would you like to see this work in real time? Download our TBD:POSTMAN COLLECTION and follow this workflow in our sandbox. 


## Reference

Want some more in-depth information about these calls, check out the API reference: https://monite.stoplight.io/docs/api-docs/YXBpOjI4MTYzMjc2-api-for-partners.  

    