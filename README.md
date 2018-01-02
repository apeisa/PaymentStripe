# PaymentStripe
Stripe payment module for ProcessWire

## Requirements
Requires PaymentModule -module.

## Example

```PHP

// Load the module and setup payment
$payment = $modules->get("PaymentStripe");
$payment->setCurrency("EUR");
$payment->setId(123456789);

$customer = Array();
$customer['email'] = "antti.peisa@gmail.com";
$payment->setCustomerData($customer);

$quantity = 1; // number of items of this kind
$amount = 1000; // Amount in payment modules always in cents
$payment->addProduct("My product", $amount, $quantity);

// In this example we are going to do all in same page
$url = $page->httpUrl;
$payment->setProcessUrl($url . "?step=process");
$payment->setFailureUrl($url . "?step=fail");
$payment->setCancelUrl($url . "?step=cancel");

switch ($input->get->step) {
	case 'process':
		$charge = $payment->processPayment();
		if ($charge) {
			echo "Thanks, payment successful!";
			// access and echo Stripe charge response
			echo $charge->id;
		} else {
			echo "Are you kidding me?";
		}
		break;

	case 'fail':
		echo "Something went wrong";
		break;

	case 'cancel':
		echo "I think you cancelled?";
		break;

	default:
		echo $payment->render(); // Here you could look if instance is PaymentEmbed or PaymentRedirect and choose method based on that
		break;
}
```

## License
GPL 2.0
