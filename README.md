# code for PSR Standard
<?php
// side effect: change ini setting
ini_set('error_reporting', E_All);

// side effect: loads a file
include "file.php";

// side effect : generate output
echo "<html>\n";

// declaration
function foo()
{
//function body
}

//this following example is of a file that contains declaration without side effects an example what to emulate

<?php
//declartion
function foo()
{
 //function body
 }
 
 conditional declartion is not a side effect
 if (! function_exist('bar')){
 
    //function body
    }
  }
  
 example of PSR coding standard
  
  public function my_method()
{
    // do something
}


public function myMethod()
{
    // do something
}


                                             4 )//create a simple payment  surcharge extenssion//





a)

firstly go to store config 
to configuer extra fee  extension setting please go to 
stores->configuration->Mageprice->Extrafee
enable disable extra fees
enable /disable calculation of extra fees 
creadit memo enable/disable refund extra fees 
select tax class
checkout total sort order to show extra fees after or before subtotal shipping discount tax
go to cart page
eextra fees  and extra fees description
go to genral specify label of extra fees which wwill be displayed on cart checkout order invoice creadit memo sales email and Pdf in descreption of extra fees which will be displayed on cart and checkout page
go to free setting select specific tax class for extra fees
go to conditions you can create condition based shipping method poscode region state country
calculate extra fees for specific category
calculate extra fees for specific product
go to conditions create conditions based on product sub selection cart attributes and condition combination
go to fee setting calculate extra fees based on specific store view and customer group
   
  
 
                                              b) fee can be fixed  or percentage  base on total amount     
                                              
                                              
Preconditions
Magento 2.2.2
PHP Version 7.0.22
Steps to reproduce
1. Create new cart price rules with the following configuration

Rule 1:

Rule Name: 10% Fix Discount
Coupon: No Coupon
Conditions: Apply for all product
Action: 
    Apply: Percent of product price discount
    Discount Amount: 10

Rule 2:

Rule Name: 5% Fix Discount
Coupon: No Coupon
Conditions: Apply for all product
Action: 
    Apply: Percent of product price discount
    Discount Amount: 5
2. Add product to cart with has price = $45

Expected result
Total discount applied $6.75
First Rule(10% of product price) Discount: (45*10) / 100 = 4.5
Second Rule(5% of product price) Discount: (45*5) / 100 = 2.25
Total Discount = 2.25 + 4.5 = 6.75
Actual result
Total discount applied $6.53
shopping cart



(d)
<?xml version="1.0"?>

<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Sales:etc/sales.xsd">
    <section name="quote">
        <group name="totals">

            <item name="fee" instance="Sugarcode\Test\Model\Total\Fee" sort_order="150"/>

        </group>  
    </section>
</config>
app\code\Sugarcode\Test\view\frontend\web\js\view\checkout\cart\totals\fee.js

/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
define(
    [
        'Sugarcode_Test/js/view/checkout/summary/fee'
    ],
    function (Component) {
        'use strict';

        return Component.extend({

            /**
             * @override
             */
            isDisplayed: function () {
                return true;
            }
        });
    }
);
app\code\Sugarcode\Test\view\frontend\web\js\view\checkout\summary\fee.js

/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
/*jshint browser:true jquery:true*/
/*global alert*/
define(
    [
        'Magento_Checkout/js/view/summary/abstract-total',
        'Magento_Checkout/js/model/quote',
        'Magento_Catalog/js/price-utils',
        'Magento_Checkout/js/model/totals'
    ],
    function (Component, quote, priceUtils, totals) {
        "use strict";
        return Component.extend({
            defaults: {
                isFullTaxSummaryDisplayed: window.checkoutConfig.isFullTaxSummaryDisplayed || false,
                template: 'Sugarcode_Test/checkout/summary/fee'
            },
            totals: quote.getTotals(),
            isTaxDisplayedInGrandTotal: window.checkoutConfig.includeTaxInGrandTotal || false,
            isDisplayed: function() {
                return this.isFullMode();
            },
            getValue: function() {
                var price = 0;
                if (this.totals()) {
                    price = totals.getSegment('fee').value;
                }
                return this.getFormattedPrice(price);
            },
            getBaseValue: function() {
                var price = 0;
                if (this.totals()) {
                    price = this.totals().base_fee;
                }
                return priceUtils.formatPrice(price, quote.getBasePriceFormat());
            }
        });
    }
);
app\code\Sugarcode\Test\view\frontend\web\template\checkout\summary\fee.html

<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<!-- ko -->

  <tr class="totals fee excl">
        <th class="mark" scope="row">
            <span class="label" data-bind="text: title"></span>
            <span class="value" data-bind="text: getValue()"></span>
        </th>
        <td class="amount">

            <span class="price"
                  data-bind="text: getValue(), attr: {'data-th': title}"></span>


        </td>
    </tr>   

<!-- /ko -->
app\code\Sugarcode\Test\view\frontend\web\template\checkout\cart\totals\fee.html

<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<!-- ko -->
<tr class="totals fee excl">
    <th class="mark" colspan="1" scope="row" data-bind="text: title"></th>
    <td class="amount">
        <span class="price" data-bind="text: getValue()"></span>
    </td>
</tr>
<!-- /ko -->
app\code\Sugarcode\Test\Model\Total\Fee.php

<?php
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
namespace Sugarcode\Test\Model\Total;


class Fee extends \Magento\Quote\Model\Quote\Address\Total\AbstractTotal
{
   /**
     * Collect grand total address amount
     *
     * @param \Magento\Quote\Model\Quote $quote
     * @param \Magento\Quote\Api\Data\ShippingAssignmentInterface $shippingAssignment
     * @param \Magento\Quote\Model\Quote\Address\Total $total
     * @return $this
     */
    protected $quoteValidator = null; 

    public function __construct(\Magento\Quote\Model\QuoteValidator $quoteValidator)
    {
        $this->quoteValidator = $quoteValidator;
    }
  public function collect(
        \Magento\Quote\Model\Quote $quote,
        \Magento\Quote\Api\Data\ShippingAssignmentInterface $shippingAssignment,
        \Magento\Quote\Model\Quote\Address\Total $total
    ) {
        parent::collect($quote, $shippingAssignment, $total);


        $exist_amount = 0; //$quote->getFee(); 
        $fee = 100; //Excellence_Fee_Model_Fee::getFee();
        $balance = $fee - $exist_amount;

        $total->setTotalAmount('fee', $balance);
        $total->setBaseTotalAmount('fee', $balance);

        $total->setFee($balance);
        $total->setBaseFee($balance);

        $total->setGrandTotal($total->getGrandTotal() + $balance);
        $total->setBaseGrandTotal($total->getBaseGrandTotal() + $balance);


        return $this;
    } 

    protected function clearValues(Address\Total $total)
    {
        $total->setTotalAmount('subtotal', 0);
        $total->setBaseTotalAmount('subtotal', 0);
        $total->setTotalAmount('tax', 0);
        $total->setBaseTotalAmount('tax', 0);
        $total->setTotalAmount('discount_tax_compensation', 0);
        $total->setBaseTotalAmount('discount_tax_compensation', 0);
        $total->setTotalAmount('shipping_discount_tax_compensation', 0);
        $total->setBaseTotalAmount('shipping_discount_tax_compensation', 0);
        $total->setSubtotalInclTax(0);
        $total->setBaseSubtotalInclTax(0);
    }
    /**
     * @param \Magento\Quote\Model\Quote $quote
     * @param Address\Total $total
     * @return array|null
     */
    /**
     * Assign subtotal amount and label to address object
     *
     * @param \Magento\Quote\Model\Quote $quote
     * @param Address\Total $total
     * @return array
     * @SuppressWarnings(PHPMD.UnusedFormalParameter)
     */
    public function fetch(\Magento\Quote\Model\Quote $quote, \Magento\Quote\Model\Quote\Address\Total $total)
    {
        return [
            'code' => 'fee',
            'title' => 'Fee',
            'value' => 100
        ];
    }

    /**
     * Get Subtotal label
     *
     * @return \Magento\Framework\Phrase
     */
    public function getLabel()
    {
        return __('Fee');
    }
}
app\code\Sugarcode\Test\etc\module.xml

<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../lib/internal/Magento/Framework/Module/etc/module.xsd">
    <module name="Sugarcode_Test" setup_version="2.0.6" schema_version="2.0.6">
        <sequence>
            <module name="Magento_Sales"/>
            <module name="Magento_Quote"/>
            <module name="Magento_Checkout"/>
        </sequence>
    </module>
</config>
app\code\Sugarcode\Test\view\frontend\layout\checkout_cart_index.xml

<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="checkout.cart.totals">
            <arguments>
                <argument name="jsLayout" xsi:type="array">
                    <item name="components" xsi:type="array">
                        <item name="block-totals" xsi:type="array">
                            <item name="children" xsi:type="array">


                                <item name="fee" xsi:type="array">
                                    <item name="component"  xsi:type="string">Sugarcode_Test/js/view/checkout/cart/totals/fee</item>
                                    <item name="sortOrder" xsi:type="string">20</item>
                                    <item name="config" xsi:type="array">
                                         <item name="template" xsi:type="string">Sugarcode_Test/checkout/cart/totals/fee</item>
                                        <item name="title" xsi:type="string" translate="true">Fee</item>
                                    </item>
                                </item>

                            </item>
                        </item>
                    </item>
                </argument>
            </arguments>
        </referenceBlock>
    </body>
</page>
app\code\Sugarcode\Test\view\frontend\layout\checkout_index_index.xml

<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" layout="1column" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="checkout.root">
            <arguments>
                <argument name="jsLayout" xsi:type="array">
                    <item name="components" xsi:type="array">
                        <item name="checkout" xsi:type="array">
                            <item name="children" xsi:type="array">

                                <item name="sidebar" xsi:type="array">
                                    <item name="children" xsi:type="array">
                                        <item name="summary" xsi:type="array">
                                            <item name="children" xsi:type="array">
                                                <item name="totals" xsi:type="array">
                                                    <item name="children" xsi:type="array">
                                                       <item name="fee" xsi:type="array">
                                                            <item name="component"  xsi:type="string">Sugarcode_Test/js/view/checkout/cart/totals/fee</item>
                                                            <item name="sortOrder" xsi:type="string">20</item>
                                                            <item name="config" xsi:type="array">
                                                                 <item name="template" xsi:type="string">Sugarcode_Test/checkout/cart/totals/fee</item>
                                                                <item name="title" xsi:type="string" translate="true">Fee</item>
                                                            </item>
                                                        </item>
                                                    </item>
                                                </item>
                                                <item name="cart_items" xsi:type="array">
                                                    <item name="children" xsi:type="array">
                                                        <item name="details" xsi:type="array">
                                                            <item name="children" xsi:type="array">
                                                                <item name="subtotal" xsi:type="array">
                                                                    <item name="component" xsi:type="string">Magento_Tax/js/view/checkout/summary/item/details/subtotal</item>
                                                                </item>
                                                            </item>
                                                        </item>
                                                    </item>
                                                </item>
                                            </item>
                                        </item>
                                    </item>
                                </item>
                            </item>
                        </item>
                    </item>
                </argument>
            </arguments>
        </referenceBlock>
    </body>
</page>
app\code\Sugarcode\Test\view\frontend\layout\sales_order_view.xml

<?xml version="1.0"?>
<!--
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">

    <body>        
        <referenceContainer name="order_totals">
            <block class="Sugarcode\Test\Block\Sales\Order\Fee" name="fee"/>
        </referenceContainer>
    </body>
</page>
app\code\Sugarcode\Test\Block\Sales\Order\Fee.php

<?php
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */

/**
 * Tax totals modification block. Can be used just as subblock of \Magento\Sales\Block\Order\Totals
 */
namespace Sugarcode\Test\Block\Sales\Order;



class Fee extends \Magento\Framework\View\Element\Template
{
    /**
     * Tax configuration model
     *
     * @var \Magento\Tax\Model\Config
     */
    protected $_config;

    /**
     * @var Order
     */
    protected $_order;

    /**
     * @var \Magento\Framework\DataObject
     */
    protected $_source;

    /**
     * @param \Magento\Framework\View\Element\Template\Context $context
     * @param \Magento\Tax\Model\Config $taxConfig
     * @param array $data
     */
    public function __construct(
        \Magento\Framework\View\Element\Template\Context $context,
        \Magento\Tax\Model\Config $taxConfig,
        array $data = []
    ) {
        $this->_config = $taxConfig;
        parent::__construct($context, $data);
    }

    /**
     * Check if we nedd display full tax total info
     *
     * @return bool
     */
    public function displayFullSummary()
    {
        return true;
    }

    /**
     * Get data (totals) source model
     *
     * @return \Magento\Framework\DataObject
     */
    public function getSource()
    {
        return $this->_source;
    } 
    public function getStore()
    {
        return $this->_order->getStore();
    }

      /**
     * @return Order
     */
    public function getOrder()
    {
        return $this->_order;
    }

    /**
     * @return array
     */
    public function getLabelProperties()
    {
        return $this->getParentBlock()->getLabelProperties();
    }

    /**
     * @return array
     */
    public function getValueProperties()
    {
        return $this->getParentBlock()->getValueProperties();
    }

    /**
     * Initialize all order totals relates with tax
     *
     * @return \Magento\Tax\Block\Sales\Order\Tax
     */
     public function initTotals()
    {

        $parent = $this->getParentBlock();
        $this->_order = $parent->getOrder();
        $this->_source = $parent->getSource();

        $store = $this->getStore();

        $fee = new \Magento\Framework\DataObject(
                [
                    'code' => 'fee',
                    'strong' => false,
                    'value' => 100,
                    //'value' => $this->_source->getFee(),
                    'label' => __('Fee'),
                ]
            );

            $parent->addTotal($fee, 'fee');
           // $this->_addTax('grand_total');
            $parent->addTotal($fee, 'fee');


            return $this;
    }

}


e) after completing order go to system in order configuration
then walltekk extensions and check order-extra fees
Define amount will be applied on every order
if extra fees will be automatically added with order
then configuration has been saved
you can add extra fees on admin side
add order extra fees
go to my account
check order extra fees
