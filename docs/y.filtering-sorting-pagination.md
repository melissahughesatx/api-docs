# Filtering, Sorting and Pagination
API consumer may affect what and how data is returned on their requests by using 3 technics:
- pagination
- sorting
- filtering

## Pagination
Pagination is a process of dividing resulting set of data into discrete blocks of information (pages). It decreases both server payload and network traffic, and increases the speed of page loading. For example, in Monite you may have 1000 Payables registered and want to receive them by groups of 1-100, where 100 is the maximum of documents that can be requested at 1 time.

### Pagination cursor field
For pagination to work, one field (column) in the database table must be selected as a pagination cursor field. By default, the pagination cursor uses `created_at` field. Every database table has such a field, so no need to specify it explicitly, unlike other cursor fields. Allowed pagination options for the cursor fields are defined for each endpoint independently, see details in [swagger specifications](YXBpOjI1NjU5MjUw-api-for-entities).

Pagination can be based on just one cursor field and this field cannot be changed during the pagination process. Let's say we have a table with `id`, `first_name`, `last_name`, `email, phone`, `created_at` fields. If the pagination is initiated with the `first_name` field, API consumer won't be allowed to modify the cursor field in/for the second page. 

### Page size (limit)
To avoid server and network overloading, the number of returned records in GET requests are restricted. In API requests the `limit` query parameter defines the number of records that API consumer want to receive in response. The miminum value is 1, the maximum is 100. Beyond this scope, an HTTP error 416 (Requested range not satisfiable) is raised.

## Sorting
As well as selecting a pagination cursor field, an API consumer may define the order in which selected records are sorted: ascendant or descendant. This order is defined by a `query` parameter of API request. The parameter may accept one of two values: `asc` (ascending) and `desc` (descending). The `asc` value is used by default.

## Filtering
Very often API consumer may want to retrieve only selected records rather than all available set of records. For example, a user is interested only in Payables created 10 days ago or later. To achive this, an API request must be extended with filter parameters. A filter is a string added into query and consisting of five parts:
1. a field name to filter the result on (e.g. `created_at`)
2. two undescores (`__`)
3. a filter condition (see below)
4. the `=` sign
5. a filter value

For instance, the filter `created_at__gte=2021-10-05T07:52:42.36638` may be used as a query parameter.
You may combine several filters on different fields with `&` (logical AND) operator. 

### Available filter conditions
- iexact (case insensitive equals)
- gte - greater or equals than passed value
- gt - greater than passed value
- lte - lower or equals than passed value
- lt - lower than passed value
- isnull - field is null (true or false)

## Example of the queries

Below are examples of API requests:
- No filters; Only cursor, sorting and limit fields are specified: `GET /v1/reconciliation?limit=10&cursor_field=was_created_by_user_name&order=desc`
- Filtering by date and limiting the result set: `GET /v1/reconciliation?limit=2&cursor_field=was_created_by_user_name&created_at__gte=2021-10-05T07:52:42.36638` 
- Filtering by two fields with default pagination: `GET /v1/reconciliation?limit=10&was_created_by_user_name=Andrey&status=active`

## Example of a response
If the data is selected based on the specified conditions, the response will look like:
```json
{
  'data': 'array of records',
    "prev_page": "/entity_users/v1/reconciliation?pagination_token=bGltaXQ9MiZmaXJzdF9vaWQ9MSZwcmV2X3Rva2VuPTM=",
    "next_page": "/entity_users/v1/reconciliation?pagination_token=bGltaXQ9MiZmaXJzdF9vaWQ9MSZuZXh0X3Rva2VuPTQ="
}
``` 
where links specified in `prev_page` and `next_page` attributes can be used to retrieve the previous or next page in the result set.

## Pagination error codes

When something goes wrong with pagination or filtering, one of the following HTTP response codes is returned:
- 406 Not Acceptable
- 416 Range Not Satisfiable