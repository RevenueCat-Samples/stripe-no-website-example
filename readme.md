### Note

This example assumes some previous knowledge about the RevenueCat platform. 

If you're looking for official [iOS](https://github.com/RevenueCat/purchases-ios), [Android](https://github.com/RevenueCat/purchases-android), [Flutter](https://github.com/RevenueCat/purchases-flutter), and other SDK's + sample apps for RevenueCat, head over to the [RevenueCat](https://github.com/RevenueCat) GitHub page.

# Stripe + RevenueCat No Website Example

RevenueCat supports web payments through Stripe. This allows you to let users subscribe on your own website, and automatically unlock access to the same subscription content through the Purchases SDK.

If you don't have or don't want a full website, it can be unclear on how to proceed with integrating Stripe payments. This sample project demonstrates how to use Stripe Checkout and webhooks to send purchase data to RevenueCat.

## Getting Started

The easiest way to use this project is by deploying to Heroku instantly with the button below.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/RevenueCat-Samples/stripe-no-website-example)

You can run the project on any server that supports Node.JS 14.x with minimal effort. 

Note: If you don't use Heroku, you'll need to swap out the herokuapp.com server URL's below with your own server.

### Environment Variables

The following environment variables should be set before starting the server.

- `RC_API_KEY` - *Required*. API key for your RevenueCat app.

- `STRIPE_KEY` - *Required*. Production Stripe API key.

- `STRIPE_KEY_TEST` - *Required*. Test Stripe API key.

- `TEST_MODE` - *Required*. Should be `true` for production mode. Set to `false` if you want use Stripe in test mode.

- `SUCCESS_URL` - *Required*. The URL that Stripe will redirect the user to after a successful purchase. If you don't have a website, you could use a URL scheme to redirect back to your mobile app.

- `CANCEL_URL` - *Required*. The URL that Stripe will redirect the user to after a user cancels a purchase without completing it. If you don't have a website, you could use a URL scheme to redirect back to your mobile app.

## Using the Sample

⚠️ **Please make sure you have [connected your Stripe account](https://docs.revenuecat.com/docs/stripe#1-connect-with-stripe) to your RevenueCat account before making test purchases.**

### Creating a Product in Stripe

Follow the instructions in our [docs](https://docs.revenuecat.com/docs/stripe-products) to create subscription products in Stripe.

### Setting Up Webhooks

You'll need to create a new webhook in Stripe to forward purchase data to your instance and subsequently to RevenueCat.

[Add a new webhook](https://dashboard.stripe.com/test/webhooks) in Stripe for the event types:

- `checkout.session.completed`

The URL for your webhook should be the following:

`http://(herokuID).herokuapp.com/webhooks/stripe`


### Purchasing

To redirect your users to a checkout, you create a `purchase` link, with the following format:

`http://(herokuID).herokuapp.com/purchase/:appUserID/:stripePriceID`

Parameters:

- `:appUserID` - The RevenueCat app user ID to which this purchase should be applied.

- `:stripePriceID` - The Price ID for the purchase. When you create a subscription with Stripe, you create the 'Product' (for example, 'Pro Mode'), then a 'Price' (for example, $15/monthly). This parameter should be the price ID. Looks something like: `price_1GyCuXCc12BVHqV1Qx5qhFXW`

After the purchase is completed, Stripe will send a webhook to this server with the data from the purchase. The server will then forward it to RevenueCat and pair it with the user ID who made the purchase.

## Post-purchase

### Finding users from the dashboard

A limitation with this example is that each transaction will be logged as a separate customer with no apparent connection between purchases in the RevenueCat dashboard and customers in the Stripe dashboard. 

To find a user's purchase in the Stripe dashboard, i.e. for support purposes, you'll need the user to send you the email they used for the purchase or the invoice number they received in their email after the purchase was complete. 

Any user can enter any email during checkout, so you'll want to be sure you've got the right invoice in the Stripe dashboard before granting any refunds.

Additionally, you could find the time of purchase from the RevenueCat dashboard and search Stripe for transactions that occurred at that time. This is not ideal and won't scale well, but it's possible for low volume apps.

