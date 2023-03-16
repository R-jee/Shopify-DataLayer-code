# Shopify-DataLayer-code

```
<script>
  var gtmId = "GTM-P2LDDTT";

  (function() {
    window.dataLayer = window.dataLayer || [];
    dataLayer.push({ ecommerce: null });

    // Prevent double-tracking if an upsell is seen or the order status page has been seen
    {% if first_time_accessed == true and post_purchase_page_accessed == false %}
    var checkout = window.Shopify.checkout;
    dataLayer.push({
      event: "dl_purchase",
      ecommerce: {
        transaction_id: "{{ order.order_number }}",
        affiliation: "{{ shop.url }}",
        coupon: getDiscountAmount(),
        currency: checkout.total_price_set.presentment_money.currency_code,
        items: items(),
        shipping: checkout.shipping_rate.price,
        tax: checkout.total_tax,
        value: checkout.total_price_set.presentment_money.amount
      }
    });

    function getDiscountAmount(line_item_count) {
      if (checkout.discount === null || typeof checkout.discount === 'undefined') return '0';
      if (checkout.discount.length === 0) return '0';
      if (line_item_count === undefined) {
        return checkout.discount.amount;
      } else {
        if (checkout.line_items[line_item_count].discount_allocations[0].amount === null || typeof checkout.line_items[line_item_count].discount_allocations[0].amount === 'undefined') return '0';
        if (checkout.line_items[line_item_count].discount_allocations[0].amount === 0) return '0';
        return checkout.line_items[line_item_count].discount_allocations[0].amount;
      };
    };

    function items(){
      var jsonarray = [];
      for (var line_item_count = 0; line_item_count < checkout.line_items.length; line_item_count++){
        var jsonobj = {
          item_id: checkout.line_items[line_item_count].product_id,
          item_name: checkout.line_items[line_item_count].title,
          currency: checkout.total_price_set.presentment_money.currency_code,
          discount: getDiscountAmount(line_item_count),
          price: checkout.line_items[line_item_count].line_price,
          quantity: checkout.line_items[line_item_count].quantity
        };
        jsonarray.push(jsonobj)
      };
      return jsonarray;
    };
    {% endif %}

    // Google Tag Manager
    (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer',gtmId);
  })();
</script>


```

### other 
```
{% if first_time_accessed %}<script>                               
	window.dataLayer = window.dataLayer || [];                                            
	var shipping_price = '{{shipping_price | money_without_currency }}';
	shipping_price  = shipping_price.replace(",", ".");
	var total_price = '{{total_price | money_without_currency }}';
	total_price  = total_price.replace(",", ".");
	var tax_price = '{{tax_price | money_without_currency }}';
	tax_price  = tax_price.replace(",", ".");
	window.dataLayer.push({
	'page_type': 'purchase',
	'event': 'websensepro_purchase',
	'currency': "{{ shop.currency }}",
	'totalValue': total_price,
	'shipping': shipping_price,
	'tax': tax_price,
	'payment_type': '{{order.transactions[0].gateway}}',
	'transaction_id': "{{order.name}}",
	});</script> 
	{% endif %}
```
