### 如何修改woocommerce的模板
http://doc.weixiaoduo.com/woocommerce/10475.html


我的账户  页面，里面有链接，需要更改IP域名。

### 一
修改了 You must be logged in to checkout.
当没有登录，加了商品到购物车，然后去结算时，就会出现，让你先登录再结算。 修改下面的文件。
```
/var/www/html/wordpress/wp-content/themes/estore/woocommerce/checkout# vi form-checkout.php 
28行
```
注意，这里需要 修改登录 的链接。 

#### estore  修改（结算-->下单）界面
```
/var/www/html/wordpress/wp-content/themes/estore/woocommerce/checkout#  vi form-billing.php 
```
```php
    <?php if ( wc_ship_to_billing_address_only() && WC()->cart->needs_shipping() ) : ?>
        <h3><?php _e( 'Billing &amp; Shipping', 'estore' ); ?></h3>
    <?php else : ?>
        <!-- modified by eeee 2018-02-21 --------------  -->
        <!-- <h3><?php _e( '账单详情', 'estore' ); ?></h3> -->
        <h3>账单详情</h3>
    <?php endif; ?>
```
在 /var/www/html/wordpress/wp-content/themes/estore/function.php文件里：修改了unset的项，会在 下单界面中体现。去掉了 "姓氏" "国家" ...
```php
// ---------------------------------------------------------------------------------------------
function custom_override_checkout_fields( $fields ) {
  //unset($fields['order']['order_comments']);
  unset( $fields['billing']['billing_country'] );
  //unset( $fields['billing']['billing_first_name'] );
  unset( $fields['billing']['billing_last_name'] );
  unset( $fields['billing']['billing_company'] );
  unset( $fields['billing']['billing_address_1'] );
  unset( $fields['billing']['billing_address_2'] );
  unset( $fields['billing']['billing_city'] );
  unset( $fields['billing']['billing_state'] );
  unset( $fields['billing']['billing_postcode'] );
  //unset($fields['billing']['billing_email']);
  unset( $fields['billing']['billing_phone'] );
return $fields;
}
add_filter( 'woocommerce_checkout_fields' , 'custom_override_checkout_fields' );
//-----------------------------------------------------------------------------------------
```

------------------------------------------------------------------------------------------------

### 二（这里修改了 woocommerce 插件)
#### 我的账户登录界面修改
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/templates/myaccount# vi form-login.php
```
#### 修改了用户 “账户详情” 修改界面！
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/templates/myaccount# vi form-edit-account.php
```
#### 修改线下支付 
```
/var/www/html/wordpress/wp-content/plugins/woocommerce/includes/gateways/cod/class-wc-gateway-cod.php
```
在里面增加了：
```php
    protected function setup_properties() {
        $this->id                 = 'cod';
        /* $this->icon               = apply_filters( 'woocommerce_cod_icon', '' ); */
        $this->icon = plugins_url('../cheque/images/wechatpay.png', __FILE__);
        $this->method_title       = __( 'Cash on delivery', 'woocommerce' );
        $this->method_description = __( 'Have your customers pay with cash (or by other means) upon delivery.', 'woocommerce' );
        $this->has_fields         = false;
    }
```

### 三
```
/var/www/html/wordpress/wp-content/themes/estore# vi header.php
```
这个文件里 修改了登录链接

### 四 去掉了 ThemeGrill Demo Importer 推荐

```
/var/www/html/wordpress/wp-content/themes/estore# vi ./inc/tgm-plugin-activation/tgmpa-estore.php
```
