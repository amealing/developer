# Customers

The customer endpoint is used to obtain a list of customers ("Plans" in the MoonClerk web interface) and their subscription info.

_There is a slight difference interminology between the MoonClerk web UI and the API. The web UI uses the term "Plan" to refer to the combination of a customer, subscription, and plan. The MoonClerk API is much more inline with the underlying Stripe contructs. In order to query the list of "plans" that one sees in the MoonClerk web UI, you would use the `/customers` endpoint._

## The Customer Object

```json
{
  "id": 523425,
  "account_balance": 0,
  "name": "Ryan Wood",
  "email": "ryan@moonclerk.com",
  "card_last4": "4242",
  "card_type": "Visa",
  "card_exp_month": 12,
  "card_exp_year": 2018,
  "customer_reference": "cus_4SOZuEc4cxP5L7",
  "discount": {
    "coupon": {
        "code": "10off",
        "duration": "once",
        "amount_off": 1000,
        "currency": "USD",
        "percent_off": null,
        "duration_in_months": null,
        "max_redemptions": null,
        "redeem_by": null
    },
    "starts_at": "2013-04-12T20:05:37Z",
    "ends_at": "2013-05-12T20:05:37Z"
  },
  "delinquent": false,
  "custom_fields": {
    "shirt_size": {
      "type": "string",
      "response": "XL"
    },
    "shipping_address": {
      "type": "address",
      "response": {
        "id": 32,
        "line1": "123 Main St.",
        "line2": "Ste. 153",
        "city": "Greenville",
        "state": "SC",
        "postal_code": "29651",
        "country": "United States"
      }
    }
  },
  "form_id": 101,
  "checkout": {
    "date": "2014-07-23T13:44:12Z",
    "subtotal": 1000,
    "fee": 200,
    "upfront_amount": 500,
    "total": 1700,
    "coupon_amount": 0,
    "amount_due": 1700,
    "trial_period_days": null
  },
  "subscription": {
    "id": 98,
    "subscription_reference": "sub_3oLgqlp4MgTZC3",
    "status": "active",
    "start": "2014-07-23T13:44:16Z",
    "first_payment_attempt": "2014-07-23T13:44:16Z",
    "next_payment_attempt": "2014-08-23T13:44:16Z",
    "current_period_start": "2014-07-23T13:44:16Z",
    "current_period_end": "2014-08-23T13:44:16Z",
    "trial_start": null,
    "trial_end": null,
    "trial_period_days": null,
    "expires_at": null,
    "canceled_at": null,
    "ended_at": null,
    "plan": {
      "id": 131,
      "plan_reference": "131",
      "amount": 1200,
      "currency": "USD",
      "interval": "month",
      "interval_count": 1
    }
  }
}
```

Notes:

* `discount` may be null if there is no current discount
* Custom field keys are set in the *Additional Information* section of the payment form builder. You can change them
* An key ending in `_reference` corresponds to a related Stripe ID

## List Customers

* `GET /customers` will return all customers

`https://api.moonclerk.com/customers`

```json
{
  "customers": [
    {
      "id": 523425,
      "name": "Ryan Wood",
      "email": "ryan@moonclerk.com",
      ...
    },
    {
      "id": 523458,
      "name": "Dodd Caldwell",
      "email": "dodd@moonclerk.com",
      ...
    },
    ...
  ]
}
```

## List Customers from a Specific Form

* `GET /forms/:form_id/customers` will return all customers for the specified form.

`https://api.moonclerk.com/forms/1234/customers`

```json
{
  "customers": [
    {
      "id": 523425,
      "name": "Ryan Wood",
      "email": "ryan@moonclerk.com",
      ...
    },
    {
      "id": 523458,
      "name": "Dodd Caldwell",
      "email": "dodd@moonclerk.com",
      ...
    },
    ...
  ]
}
```


## Get a Customer

* `GET /customers/:id` will return the specified customer.

`https://api.moonclerk.com/customers/523425`

```json
{
  customer: {
    "id": 523425,
    "name": "Ryan Wood",
    "email": "ryan@moonclerk.com",
    ...
  }
}
```