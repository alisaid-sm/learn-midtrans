# Learn Midtrans payment gateway in express.js

### Reference

- https://docs.midtrans.com/en/technical-reference/sandbox-test?id=testing-payment-on-sandbox

- https://docs.midtrans.com/en/after-payment/http-notification?id=example-on-handling-http-notifications

- https://github.com/Midtrans/midtrans-nodejs-client/tree/master/examples/expressApp

### Flow Payment Gateway

- Using Snap
1. install midtrans client ``` npm i midtrans-client --save```
2. create src/utils/midtrans.js
```
  let snap = new midtransClient.Snap({
    isProduction : false, when your application will go to production change this to true
    serverKey : SERVER_KEY,
    clientKey : CLIENT_KEY
  });

  module.exports = snap
```
3. tugas dari backend adalah mengirimkan token hasil dari data yang di dapat dari body(parameter)
```
  let parameter = {
    "transaction_details": {
      "order_id": "order-id-node-"+Math.round((new Date()).getTime() / 1000),
      "gross_amount": 200000
    }, "credit_card":{
      "secure" : true
    }
  };
  // create snap transaction token
  snap.createTransactionToken(parameter)
    .then((transactionToken)=>{
        // pass transaction token to frontend
        res.json({
          token: transactionToken, 
          clientKey: snap.apiConfig.clientKey
        })
    })
```
4. di frontend entah pake tech stack apa
```
      checkoutBtn.onclick = function(){
        console.log('opening snap popup:');
        
        // Open Snap popup with defined callbacks.
        snap.pay(token, {
          onSuccess: function(result) {
            console.log("SUCCESS", result);
            alert("Payment accepted \r\n"+JSON.stringify(result));
          },
          onPending: function(result) {
            console.log("Payment pending", result);
            alert("Payment pending \r\n"+JSON.stringify(result));
          },
          onError: function() {
            console.log("Payment error");
          }
        });
```

5. nanti akan otomatis muncul popup informasi pembayaran lengkap

![image](https://user-images.githubusercontent.com/68319083/144034955-29ae7677-2b35-449d-98d3-b8d3296f4300.png)

6. untuk test payment sandbox. pilih dulu metode pembayaran. trus pilih payment simulator di link berikut

https://docs.midtrans.com/en/technical-reference/sandbox-test?id=testing-payment-on-sandbox

7. untuk handle automatic status paid, ubah saat client get api status payment

https://docs.midtrans.com/en/after-payment/http-notification?id=example-on-handling-http-notifications

- Core Api

