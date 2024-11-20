# PaymentGatewaySwiftSDK Integration Guide

## Introduction

The `PaymentGatewaySwiftSDK` is a CocoaPod library designed to handle payments seamlessly within iOS applications. It provides easy integration with a payment gateway, allowing you to initiate payments and handle responses effectively.

This guide walks you through integrating the `PaymentGatewaySwiftSDK` into your project and using its features for payment processing.

## Installation

To install the `PaymentGatewaySwiftSDK` dependency, you need to add it to your `Podfile`:

```ruby
pod 'PaymentGatewaySwiftSDK'
```

OR

```ruby
pod 'PaymentGatewaySwiftSDK', '1.0.2'
```

## Setup

Add this code snippet to your applications `info.plist`.

```xml
<dict>
    ...
    <key>LSApplicationQueriesSchemes</key>
  	<array>
  		<string>upi</string>
  		<string>credpay</string>
  		<string>gpay</string>
  		<string>phonepe</string>
  		<string>paytmmp</string>
  		<string>mobikwik</string>
  		<string>com.amazon.mobile.shopping</string>
  		<string>bharatpe</string>
  		<string>freecharge</string>
  		<string>payzapp</string>
  		<string>myjio</string>
  		<string>bhim</string>
  		<string>slice</string>
  	</array>
    ...
</dict>
```

## Example Usage

Here is an example of how to integrate the `PaymentGatewaySwiftSDK` into your project:

### 1. Import the Library

Import the necessary modules in your `ViewController.swift`:

```swift
import PaymentGatewaySwiftSDK
```

### 2. Setup the Payment Process

The following code demonstrates how to set the `params` and `initialize` the `PaymentGatewaySwiftSDK`:

```swift
let params = PaymentGatewayParams(
            apiKey: <API_KEY>,
            orderID: <ORDER_ID>,
            theHash: <HASH>,
            mode: <MODE>,
            amount: <AMOUNT>,
            name: <NAME>,
            phone: <PHONE>,
            email: <EMAIL>,
            returnURL: <RETURN_URL>,
            theDescription: <DESCRIPTION>,
            currency: <CURRENCY>,
            country: <COUNTRY>,
            city: <CITY>,
            state: <STATE>,
            addressLine1: <ADD_1>,
            addressLine2: <ADD_2>,
            zipCode: <ZIP_CODE>
        )

let url = "<PAYMENT_URL>"
PaymentGateway.open(from: self, url: url, delegate: self, withParams: params)
```

### 3. Handle Payment Responses
Implement the delegate methods to handle payment completion or cancellation:

```swift
extension ViewController: PaymentGatewayDelegate {
    func didPaymentCompleted(_ controller: UIViewController, withData data: Any?) {
        print("Payment Successful")
        print("response Data: \(String(describing: data as? String))")
        if let jsonString = data as? String, let response = jsonString.toJSONDictionary {
            responseTextView.text = jsonString
            if let transactionId = response.value(forKey: "transaction_id") as? String {
                print("Transaction ID: \(transactionId)")
            } else {
                print("Transaction ID is missing")
            }
            print("Payment Success")
        } else {
            responseTextView.text = "Payment Failed"
            print("Payment Failure")
        }
        controller.dismiss(animated: true, completion: nil)
    }

    func didPaymentCanceled(_ controller: UIViewController) {
        controller.dismiss(animated: true, completion: nil)
        responseTextView.text = "Payment Canceled"
    }
}
```
