# Stripe + RevenueCat No Website Example

RevenueCat supports web payments through Stripe. This allows you to let users subscribe on your own website, and automatically unlock access to the same subscription content through the Purchases SDK.

If you don't have or don't want a full website, it can be unclear on how to proceed with integrating Stripe payments. This sample project demonstrates how to use Stripe Checkout and webhooks to send purchase data to RevenueCat.

## Getting Started

The easiest way to use this project is by deploying to Heroku instantly with the button below.

<!-- Heroku Button -->

You can run the project on any server that supports Node.JS 14.x with minimal effort.

### Environment Variables

The following environment variables should be set before starting the server.

`RC_API_KEY` - Required. API key for your RevenueCat app.

`STRIPE_KEY` - Required. Production Stripe API key.

`STRIPE_KEY_TEST` - Required. Test Stripe API key.

`TEST_MODE` - Required. Should be `true` for production mode. Set to `false` if you want use Stripe in test mode.

`SUCCESS_URL` - Required. The URL that Stripe will redirect the user to after a successful purchase. If you don't have a website, you could use a URL scheme to redirect back to your mobile app.

`CANCEL_URL` - Required. The URL that Stripe will redirect the user to after a user cancels a purchase without completing it. If you don't have a website, you could use a URL scheme to redirect back to your mobile app.

### Using the Sample

#### Creating a Product in Stripe

#### Setting Up Webhooks

#### Purchasing

To redirect your users to a checkout, you create a `purchase` link, with the following format:

`http://(herokuID).herokuapp.com/purchase/:appUserID/:stripePriceID`

Parameters:

`appUserID` - The RevenueCat app user ID to which this purchase should be applied.

`stripePriceID` - The Price ID for the purchase. When you create a subscription with Stripe, you create the 'Product' (for example, 'Pro Mode'), then a 'Price' (for example, $15/monthly). This parameter should be the price ID. Looks something like: `price_1GyCuXCc12BVHqV1Qx5qhFXW`

