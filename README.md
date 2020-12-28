# Samsung Pay (SPAY)

This plugin implements Samsung Pay SDK features for card enrollment integration. Lets customers add a bank card to Samsung Pay from the issuer app by providing the required card details.

## Supported Platforms

- Android (Samsung devices)

## Using
It's only documentation for preview of capabilities. If you want you this plugin, contact with author for getting full version plugin.

## Installation

Type one of this string in cordova application root with following params:
- SERVICE_ID: "SERVICE ID" value from "Service details" in "Services management"
- DEBUG_API_KEY: "DEBUG API KEY" value from "Service details" in "Services management"

For develop:
```sh
cordova plugin add cordova-samsung-provision --variable spay_service_id="SERVICE_ID" --variable debug_mode="Y" --variable spay_debug_api_key="DEBUG_API_KEY"
```

For production:
```sh
cordova plugin add cordova-samsung-provision --variable spay_service_id="SERVICE_ID" --variable debug_mode="N" --variable spay_debug_api_key=""
```

## Check Samsung Pay status
This method must be called before using any other feature in the Samsung Pay SDK.

```js
getSamsungPayStatus(successCallback, errorCallback);
```

Success callback structure:

```json
{
  "countryCode": "RU",
  "status": 2,
  "statusName": "SPAY_READY",
  "service_ID": "SERVICE_ID"
}
```

## Activate Samsung Pay
If the status returned by the check above is SPAY_NOT_READY, your app can, as desired or necessary, activate the app on the
device and update it to the latest version in release.

```js
activateSamsungPay()
```

## Update Samsung Pay
The SamsungPay furnishes the following API method to update the Samsung Pay app on a device. Call this method to activate the Samsung Pay app on the device running the partner app when the status is SPAY_NOT_READY and EXTRA_ERROR_REASON is ERROR_SPAY_NEED_TO_UPDATE.

```js
goToUpdatePage()
```

## Check allready enrolled cards
The getAllCards() method is used to request a list of the issuer's cards currently registered/enrolled by this
user in Samsung Pay. The result is limited to cards matching the ISSUER NAME(s) specified and registered to the serviceID on the portal.
The getAllCards() method is typically called when the issuer app wants to check card status. It does not need to be called every time
the partner app resumes.

```js
getAllCards(successCallback, errorCallback);
```

Success callback structure:

```json
[{
   "cardBrand": "MASTERCARD",
   "cardId": "DSAPMC0000232557e6568c354f8142bc9a8cee6c06c21e3e",
   "cardInfo": {
      "mFlags": 1536,
      "mMap": {
         "cardType": "PAYMENT",
         "deviceType": "phone",
         "issuerName": "ISSUER NAME",
         "last4Dpan": "2516",
         "last4Fpan": "5139"
      }
   },
   "cardStatus": "ACTIVE"
}]
```

## Get wallet information
The getWalletInfo() method is used to request wallet information from Samsung Pay. Partner apps use this information to identify a unique
combination of the user and the Samsung Pay app on a particular device by querying the corresponding WALLET_DM_ID(wallet
device master ID), DEVICE_ID, and WALLET_USER_ID before calling an App2App API, such as addCard().

```js
getWalletInfo(successCallback, errorCallback);
```

Success callback structure:

```json
{
  "walletDMId": "5ZrQ4gdfgHWdQj3s1j17gQ",
  "walletUserId": "PsOTBVWdTAmHcvbL9bRQpw",
  "deviceId": "MTkvMjA3MDAdNDAwMDInNaAz"
}
```

## Add a card to Samsung Pay (push provisioning)
Your mobile bank app can call the addCard() method to add a credit/debit card to Samsung Pay. First, however, call
getWalletInfo() to determine if the user's card is eligible to be added to Samsung Pay and the unique user ID and device ID
combination indicate a legitimate cardholder and registered Samsung Pay user.

> Samsung Pay does not provide detailed provisioning payload information. You must generate the provisioning payload in
  accordance with your card network/TSP specifications. Be sure to obtain the relevant issuer implementation guide and
  specifications from each respective card network to implement  required handling by your partner app and server.  

```js
addCard("TYPE", "PROVIDER", "PAYLOAD", successCallback, errorCallback);
```

## General error callback structure

```json
{
  "errorReason": -350,
  "errorReasonMessage": "ERROR_DEVICE_NOT_SAMSUNG",
  "status": 0,
  "statusName": "SPAY_NOT_SUPPORTED"
}
```

## List of card types

```json
[
  "PAYMENT",
  "GIFT",
  "LOYALTY",
  "CREDIT",
  "DEBIT",
  "TRANSIT"
]
```

## List of card providers

```json
{
  "PROVIDER_VISA": "VI",
  "PROVIDER_MASTERCARD": "MC",
  "PROVIDER_AMEX": "AX",
  "PROVIDER_DISCOVER": "DS",
  "PROVIDER_GTO": "GTO",
  "PROVIDER_PLCC": "PL",
  "PROVIDER_GIFT": "GI",
  "PROVIDER_LOYALTY": "LO",
  "PROVIDER_PAYPAL": "PP",
  "PROVIDER_GEMALTO": "GT",
  "PROVIDER_NAPAS": "NP",
  "PROVIDER_MIR": "MI",
  "PROVIDER_PAGOBANCOMAT": "PB"
}
```

## List of error codes and meanings

```json
{
  "SPAY_WATCH_UPDATE_IS_ONGOING": -702,
  "SPAY_WATCH_TAKING_LOG_FOR_REPORT": -703,
  "SPAY_READY": 2,
  "SPAY_NOT_SUPPORTED": 0,
  "SPAY_NOT_READY": 1,
  "SPAY_NOT_ALLOWED_TEMPORALLY": 3,
  "SPAY_HAS_TRANSIT_CARD": 10,
  "SPAY_HAS_NO_TRANSIT_CARD": 11,
  "ERROR_WALLET_ID_MISMATCH": -516,
  "ERROR_VERIFY_CARD": -8,
  "ERROR_USER_NOT_REGISTERED_FOR_DEBUG": -306,
  "ERROR_USER_CANCELED": -7,
  "ERROR_UNKNOWN": -999,
  "ERROR_UNAUTHORIZED_REQUEST_TYPE": -309,
  "ERROR_UNABLE_TO_VERIFY_CALLER": -359,
  "ERROR_TSM_FAIL": -507,
  "ERROR_TRANSACTION_TIMED_OUT": -111,
  "ERROR_TRANSACTION_CLOSED": -112,
  "ERROR_SPAY_WATCH_PIN_LOCK_SETUP_CANCELED": -701,
  "ERROR_SPAY_WATCH_PAY_PROGRESS": -704,
  "ERROR_SPAY_WATCH_CONNECTION": -705,
  "ERROR_SPAY_USIM_CHANGED": -603,
  "ERROR_SPAY_SETUP_NOT_COMPLETED": -356,
  "ERROR_SPAY_SDK_SERVICE_NOT_AVAILABLE": -352,
  "ERROR_SPAY_RESET": -116,
  "ERROR_SPAY_PKG_NOT_FOUND": -351,
  "ERROR_SPAY_PIN_LOCK_SETUP_CANCELED": -701,
  "ERROR_SPAY_MST_OVERSEAS_NOT_SUPPORTED": -601,
  "ERROR_SPAY_MST_NOT_PREPARED": -602,
  "ERROR_SPAY_INTERNAL": -1,
  "ERROR_SPAY_FMM_LOCK": -604,
  "ERROR_SPAY_CONNECTED_WITH_EXTERNAL_DISPLAY": -605,
  "ERROR_SPAY_APP_NEED_TO_UPDATE": -357,
  "ERROR_SPAY_APP_INTEGRITY_CHECK_FAIL": -360,
  "ERROR_SHORT_INFORMATION": -354,
  "ERROR_SHIPPING_ADDRESS_UNABLE_TO_SHIP": -202,
  "ERROR_SHIPPING_ADDRESS_NOT_EXIST": -203,
  "ERROR_SHIPPING_ADDRESS_INVALID": -201,
  "ERROR_SESSION_TIMED_OUT": -110,
  "ERROR_SESSION_LOCKED": -109,
  "ERROR_SESSION_INITATE_TIMED_OUT": -513,
  "ERROR_SESSION_INITATE_FAILED": -512,
  "ERROR_SERVICE_VERSION_NOT_SUPPORTED": -304,
  "ERROR_SERVICE_UNAVAILABLE_FOR_THIS_REGION": -302,
  "ERROR_SERVICE_NOT_VERIFIED_WITH_PARTNER": -303,
  "ERROR_SERVICE_NOT_APPROVED_FOR_RELEASE": -307,
  "ERROR_SERVICE_ID_INVALID": -301,
  "ERROR_SERVICE_BLOCKED": -305,
  "ERROR_SERVER_REJECT": -505,
  "ERROR_SERVER_NO_RESPONSE": -22,
  "ERROR_SERVER_INTERNAL": -311,
  "ERROR_SDK_NOT_SUPPORTED_FOR_THIS_REGION": -300,
  "ERROR_REGISTRATION_FAIL": -104,
  "ERROR_PRODUCT_VERSION_NOT_SUPPORTED": -304,
  "ERROR_PRODUCT_UNAVAILABLE_FOR_THIS_REGION": -302,
  "ERROR_PRODUCT_NOT_VERIFIED_WITH_PARTNER": -303,
  "ERROR_PRODUCT_NOT_APPROVED_FOR_RELEASE": -307,
  "ERROR_PRODUCT_ID_INVALID": -301,
  "ERROR_PRODUCT_BLOCKED": -305,
  "ERROR_PAYMENT_PROTOCOL_NOT_SUPPORTED": -401,
  "ERROR_PARTNER_SERVICE_TYPE": -11,
  "ERROR_PARTNER_SDK_VERSION_NOT_ALLOWED": -358,
  "ERROR_PARTNER_SDK_API_LEVEL": -10,
  "ERROR_PARTNER_NOT_APPROVED": -308,
  "ERROR_PARTNER_INFO_INVALID": -99,
  "ERROR_PARTNER_APP_VERSION_NOT_SUPPORTED": -304,
  "ERROR_PARTNER_APP_SIGNATURE_MISMATCH": -303,
  "ERROR_PARTNER_APP_NEED_TO_UPDATE": -358,
  "ERROR_PARTNER_APP_BLOCKED": -305,
  "ERROR_NO_NETWORK": -21,
  "ERROR_NOT_SUPPORTED": -3,
  "ERROR_NOT_READY_PAYMENT": -108,
  "ERROR_NOT_FOUND": -4,
  "ERROR_NOT_ALLOWED": -6,
  "ERROR_NONE": 0,
  "ERROR_MISSING_INFORMATION": -354,
  "ERROR_MAX_PAN_PROVISION_NUM_REACHED": -515,
  "ERROR_MAX_CARD_NUM_REACHED": -506,
  "ERROR_MAKING_SHEET_FAILED": -115,
  "ERROR_INVALID_PARAMETER": -504,
  "ERROR_INVALID_INPUT": -2,
  "ERROR_INVALID_CARDINPUT": -503,
  "ERROR_INVALID_CARD": -502,
  "ERROR_INTERNAL_ADDRESS_UPDATED": -114,
  "ERROR_INITIATION_FAIL": -103,
  "ERROR_FRAMEWORK_INTERNAL": -501,
  "ERROR_EXPIRED_OR_INVALID_DEBUG_KEY": -310,
  "ERROR_DUPLICATED_SDK_API_CALLED": -105,
  "ERROR_DEVICE_NOT_SAMSUNG": -350,
  "ERROR_DEVICE_INTEGRITY_CHECK_FAIL": -353,
  "ERROR_CARD_NOT_SUPPORTED_ONLINE_PAY": -402,
  "ERROR_CARD_NOT_SUPPORTED_IN_LATEST_SPAY": -402,
  "ERROR_CARD_NOT_SUPPORTED": -514,
  "ERROR_CARD_IDV_NOT_SUPPORTED": -521,
  "ERROR_CARD_ALREADY_REGISTERED": -500,
  "ERROR_BILLING_ADDRESS_NOT_EXIST": -205,
  "ERROR_BILLING_ADDRESS_INVALID": -204,
  "ERROR_AUTH_CODE_TYPE_INVALID": -520,
  "ERROR_AUTH_CODE_MAX_TRY_REACHED": -519,
  "ERROR_AUTH_CODE_INVALID": -517,
  "ERROR_AUTH_CODE_EXPIRED": -518,
  "ERROR_AUTHENTICATION_TIMED_OUT": -511,
  "ERROR_AUTHENTICATION_NOT_READY": -508,
  "ERROR_AUTHENTICATION_FAILED": -509,
  "ERROR_AUTHENTICATION_CLOSED": -510,
  "ERROR_ANDROID_PLATFORM_CHECK_FAIL": -361,
  "ERROR_ALREADY_DONE": -5,
  "ERROR_ADDRESS_UPDATED_TIME_OUT": -113
}
``` 
