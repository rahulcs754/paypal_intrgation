# paypal_intrgation
this is demo of paypal intregation


# add this file in lib


#this is javascript and html code 

```html
<!DOCTYPE html>
<head>
    <!-- Add meta tags for mobile and IE -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />

	
	 <meta name="viewport" content="width=device-width, initial-scale=1.0" />
	 <link rel="icon" href="<?php echo base_url().'assets/favi/favicon.ico'; ?>" type="image/gif" sizes="16x16">
    <link href="https://hstwebtest.com/vincere/assets/css/bootstrap.min.css" rel="stylesheet" />
    <link href="https://hstwebtest.com/vincere/assets/css/font-awesome.min.css" rel="stylesheet" />
    <link href="https://hstwebtest.com/vincere/assets/css/style.css" rel="stylesheet" />
    <link href="https://hstwebtest.com/vincere/assets/css/fullcalendar.css" rel="stylesheet" />
    <link href="https://hstwebtest.com/vincere/assets/css/toastr.min.css" rel="stylesheet" />
    <link rel="stylesheet" href="https://cdn.datatables.net/1.10.20/css/dataTables.bootstrap.min.css" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/summernote@0.8.18/dist/summernote.min.css" />
</head>

<body>
<?php
  
//sandbox
//$paypalClientID = 'AT9W81DtgnP3hZymn1cfOoqiV1Bu65vUxBbYpfDY0LEIWD-lpCYHnPmKLYVmRUuFH4Z747c-wzcKlqPd';
//live 
$paypalClientID = 'AaH7wvfcjvO0SjsAOckf7rO0_CznTQCo7LZfohGxWiFKzMjUHrREwriRs9KfLnkZq1cMOrNNJK6kr1bS';
$productDataPrice = $this->Common_Model->priceforpropertyset();
?>



<!-- Set up a container element for the button -->
<center><div id="paypal-button"></div></center> 



 <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script> 
<script src="https://hstwebtest.com/vincere/assets/js/bootstrap.min.js"></script> 
 
<script src="https://www.paypal.com/sdk/js?client-id=<?php echo $paypalClientID; ?>&currency=EUR"></script>

<script src="<?php echo base_url(); ?>assets/js/toastr.min.js"></script>

<script>
     var item_number1 = "<?php echo $this->uri->segment(2); ?>";
	 var custom = "<?php echo $this->session->userdata('id'); ?>";
     var returnurl = "<?php echo base_url(); ?>property_list";
    paypal.Buttons({
    createOrder: function(data, actions) {
      // This function sets up the details of the transaction, including the amount and line item details.
      return actions.order.create({
        purchase_units: [{
          amount: {
            currency_code: "EUR", 
            value: '<?php echo $productDataPrice; ?>'
          }
        }]
      });
    },
    onApprove: function(data, actions) {
      // This function captures the funds from the transaction.
      return actions.order.capture().then(function(details) {
		 
		console.log('Transaction completed by ' + JSON.stringify(details));
         
		 var create_date = details.create_time;
		 var txn_id = details.id;
		 var payment_status = details.status;
		 var payer_email = details.payer.email_address;
		 var payment_gross = details.purchase_units[0].amount.value;
		 var mc_currency = details.purchase_units[0].amount.currency_code;
		 
		
		if(payment_status == 'COMPLETED'){
			 
		 $.ajax({
		   url: '<?php echo base_url(); ?>paypal/payment_datasave',
		   type: "POST",
		   data: {item_number1:item_number1,custom:custom,create_date:create_date,txn_id:txn_id,payment_status:payment_status,payer_email:payer_email},
		   beforeSend: function () {
			//$("#overlay").show(); 
		   }, 
		   success: function (data) {
			 window.location = returnurl;   
			 toastr["success"]('Transaction completed');
		   } 
		 }); 
			
		    
		 }else{
			 toastr["success"]('Transaction is not completed');
			 window.location = returnurl;  
		 } 
		 

      });
    }
  }).render('#paypal-button');

</script>


<script>
toastr.options = {
  "closeButton": false,
  "debug": false,
  "newestOnTop": true,
  "progressBar": false,
  "positionClass": "toast-top-right",
  "preventDuplicates": true,
  "onclick": null,
  "showDuration": "300",
  "hideDuration": "1000",
  "timeOut": "5000",
  "extendedTimeOut": "1000",
  "showEasing": "swing",
  "hideEasing": "linear",
  "showMethod": "fadeIn",
  "hideMethod": "fadeOut"
}
 
</script>


</body>
</html>
    

```
