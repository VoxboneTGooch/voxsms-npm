# Voxbone VoxSMS

The Voxbone VoxSMS module enables you to send/receive SMS and delivery reports from Voxbone numbers.

## Install

To install the Voxbone VoxSMS module and its dependencies, simply run the following command:

`npm install voxbone-voxsms`

## How to use

### Instantiate the module
1. Add the dependency to your application

    `````
    var Voxbone = require('voxbone-voxsms');
    `````

2. Add your credentials

    ````
    var api_login = 'login';
    var api_password = 'password';
    ````

3. Create a new Voxbone object

    `````
    var voxbone = new Voxbone({
      apiLogin: api_login,
      apiPassword: api_password
    });
    `````

### Send an SMS

1. Add your parameters to send a message

    `````
    var from = "+3228080438"; //a Voxbone number enabled for VoxSMS (format: +3200000)
    var to = "3222222222"; //the destination number (format: 3200000)
    var msg = "your message";
    var dr = "none"; //Delivery reports - Accepted values: success, failure, all, none
    `````

2. Generate a fragmentation reference for your sms

    This will be used in case your message is too long.

    `````
    var fragref = voxbone.createFragRef();
    `````

3. Send SMS with the parameters configured in step 1 (success/failure callback functions are optional)

    `````
    var sms_sent = false;
    var success = false;
    
    voxbone.sendSMS(number, from_number, msg, fragref, 'all', function(transaction_id) {

        if (frag === null) {
            sms_sent = true;
	    success = true;
            console.log("Single SMS sent, transaction ID : "  + transaction_id);
        } else if (frag.frag_num === frag.frag_total) {
            sms_sent = true;
            console.log(frag.frag_num + "/" + frag.frag_total + " SMS sent OK, transaction ID: "  + transaction_id);
        } else {
            console.log(frag.frag_num + "/" + frag.frag_total + " SMS sent OK, transaction ID: "  + transaction_id);
	    }
     }, function(statusCode, frag) {
       	    success = false;
       	    if (frag) {
       	        console.log(frag.frag_num + "/" + frag.frag_total + " SMS sent OK, transaction ID: "  + transaction_id);
            } else {
	            console.log("SMS failed to be sent, error code : " + statusCode);
	        }
     });
    `````

## Docs

Available functions:

1.  Sends an SMS with the parameters configured

    ````
    voxbone.sendSMS(to, from, msg, fragref, dr[, successCB, [, failCB]]);
    ````

2.  Generates a random fragmentation reference used for long messages

    `````
    voxbone.createFragRef();
    `````

3.  Generates a transaction ID to be sent back with your 200OK response when receiving a message.

    `````
    voxbone.createTransId();
    `````

4.  Sends a delivery report to Voxbone when your application receives a message

    `````
    voxbone.sendDeliveryReport();
    `````

## Resources
* [Sample application](https://github.com/voxbone/voxsms-client-node)
* [VoxSMS API Documentation](https://developers.voxbone.com/docs/sms/overview/)
* [Voxbone Github](https://github.com/voxbone)

## License

[MIT](LICENSE)

[npm-url]: https://npmjs.org/package/voxbone-voxsms
[downloads-url]: https://npmjs.org/package/voxbone-voxsms
