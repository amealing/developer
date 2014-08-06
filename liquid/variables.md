# Liquid Variables & Filters for MoonClerk

## Account
- `account.name` -  Your MoonClerk account name
- `account.email` - You MoonClerk account email (may be different from your sign in email)
- `account.currency` - The currency your MoonClerk and Stripe account are using

## Plan (Customer)
- `plan.id` - MoonClerk Plan ID
- `plan.reference` - Stripe Customer ID for this plan
- `plan.url` - a link to this plan on MoonClerk
- `plan.payer_manage_url` - The link for the payer to manage their plan
- `plan.name` - Payer name
- `plan.email` - Payer email
- `plan.card.last4` - Last four digits of card
- `plan.card.type` - Card Type, i.e. Visa, MasterCard, etc.
- `plan.card.expiration` - The month and year the active card expires
- `plan.discount.coupon` - A description of the currently applied coupon, i.e. "10off ($10.00 off once)". This will only exist if there is an active discount. Otherwise it will be empty.
- `plan.discount.expiration` - The date the active discount expires for this plan. This will only exist if there is an active discount. Otherwise it will be empty.
- `plan.status` - The status of the plan (Active, Not Started, Canceled, etc.)
- `plan.next_payment_attempt` -  The next scheduled payment attempt on this plan
- `plan.start` - The first scheduled payment attempt
- `plan.expires` - The expiration date of the plan, if applicable
- `plan.amount` - The recurring amount in cents
- `plan.interval` - week, month or year
- `plan.interval_count` - 1 or more
- `plan.frequency` - interval phrase, i.e. "every 3 months”
- `plan.custom_fields` - A Hash of custom field responses based on the form. See [Custom Fields](#custom-fields) section.
- `plan.checkout.date` - The date of the checkout
- `plan.checkout.amount_due` - The total amount of the first payment. This is the big number the payer sees when checking out. Amount is in cents.
- `plan.checkout.subtotal` - The amount of the form or the chosen amount if deferred. Amount is in cents.
- `plan.checkout.fee` - The net fee amount based on a fee amount or percentage as configured in your form. Amount is in cents.
- `plan.checkout.upfront_amount` - The one-time upfront amount that is added to the first payment. (Fees are not applied to this amount.) Amount is in cents.
- `plan.checkout.trial_period_days` - The number of trial period days for the checkout, if applicable.

## Payment
- `payment.id` - the MoonClerk internal payment ID
- `payment.reference` - the Stripe charge ID
- `payment.url` - a link to this plan on MoonClerk
- `payment.date` - create date
- `payment.name` - Payer's name
- `payment.email` - Payer's email
- `payment.amount` - the payment amount in cents
- `payment.fee` - the fee in cents
- `payment.amount_refunded` - the amount refunded in cents
- `payment.status` - payment status in the dashboard
- `payment.card.last4` - last 4 digits of the card used
- `payment.card.type` - the card type (Visa, MasterCard,etc.)
- `payment.invoice` - the Stripe invoice ID
- `payment.custom_fields - A Hash of custom field responses based on the form. See [Custom Fields](#custom-fields) section.

## Form
- `form.id`
- `form.url`
- `form.title`
- `form.description`

## Custom Fields

Fields contain the responses from the custom fields created on your
forms. They are accessed through either `payment.custom_fields` or `plan.custom_fields`.

The `custom_fields` variable is a Hash. [See how to loop over a Hash in Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#for-loops).

The key will be the key you assign during the creation of your custom field in the payment form builder. The value contain the following fields:

- `title` - the title of the field
- `response` - the text response or the address on a single line
- `address` - contains field with the address parts. If the field is not an address it will be empty (null).

If address is not empty, it will contain:

- `address.line1`
- `address.line2`
- `address.city`
- `address.state`
- `address.postal_code`
- `address.county`

Here is a sample of the structure of some custom field data:

```
  'shipping_address' => {
    'title' => 'Shipping Address',
    'address' => {
      'line1' => '123 Sycamore St.',
      'line2' => 'Suite 100',
      'city' => 'Greenville',
      'state' => 'SC',
      'postal_code' => '29601',
      'country' => 'United States'
    },
    'response' => '123 Sycamore St., Suite 100, Greenville, SC 29601, United States'
  },
  'tshirt_size' => {
    'title' => 'T-shirt size',
    'address' => nil,
    'response' => 'X-Large'
  }
```

### Looping over fields

When looping through fields, the actual variable name will be determined
by how you name the for loop in the template. In this example, the variable
will be called `field`.

```Liquid
{% for field in payment.custom_fields %}
{{ field[1].title }}: {{ field[1].response }}
{% endfor %}
```

Liquid uses the `[1]` to access the value of the field hash.


### Accessing a specific field for display

Another way to deal with custom field is to access a specific field. It is important to remember that the notification template is across all forms. If you write code to access a certain custom field you will want it to degrade nicely if the field doesn't exist.

Let's say you have a `shipping_address` key on many of you forms and want to output it on the payment successful notification to the account holder.

```Liquid
{% if payment.custom_fields.shipping_address %}
{% assign address = payment.custom_fields.shipping_address.address %}
{{ address.line1 }}{% if address.line2 %}
{{ address.line2 }}{% endif %}
{{ address.city }}, {{ address.state }} {{ address.postal_code }}{% endif %}
```



# Liquid Filters

## MoonClerk Filters
- `money` - formats money with a symbol
- `money_with_currency` - formats money with a symbol and currency
- `money_without_currency` - formats money as a decimal

## Official Liquid Docs

Visit the [Official Liquid Wiki](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)
to learn more about the standard filters and control flow constructs (if-then, etc.)
that you can use within templates.