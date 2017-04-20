# Magento 2 EPOS Integration System

This is a custom Magento 2 module for connecting Magento 2 to an Electronic Point Of Sales (EPOS) system. It is aimed towards Magento 2 developers that are developing an ecommerce site for a client with one or more physical stores that already use an EPOS system.

This integration stops the problem of stock on the website that is already sold out and the time spent copying orders from the Magento 2 system to the clients EPOS system.

The module allows exporting of orders and importing of stock levels. The problem with this is that every EPOS system is different and requires custom development to connect. This module is not a complete system, rather it contains all the components needed to set up an EPOS integration. PHP knoweledge is requried to get this module working completely.

To simplify this development process all the functions and settings for the order export are contained in one file (Alstar/Epos/Order/OrderProcess.php) and all the functions and settings for the stock import are also in one file (Alstar/Epos/Stock/StockProcess.php). In these files detailed comments are included to help development.

Most newer EPOS systems allow for SOAP based requests and responses, using SOAP the orders would be exported and stock imported via XML using a wsdl url. Older EPOS systems use CSV files exports and imports. This module contains options for both SOAP and CSV files and the admin configuration settings allow the selection of which method to use.

> **Note: An important point to be aware of when importing stock levels is with un-processed orders in the clients EPOS system. If all the Magento 2 orders have not been imported into the clients EPOS system when the stock is exported it can lead to incorrect stock levels on the web site, e.g. Assume Magento stock levels are correct at the start of the day for product1 with a quantity of 10 and the EPOS quantity for this product is also 10. Lets say there were two orders for this product so that the Magento quantity is now 8. If the stock update is run using the EPOS values before the two orders have been processed then the stock would be reset to 10 which is incorrect. To solve this problem the module exports the orders as soon as they are placed, with a SOAP connection the clients EPOS system quantities would then always be correct. However with CSV exports the clients EPOS system must be configured to process all the CSV files before doing the stock update.**

The module adds a new column to the orders grid in the admin called 'Epos Status' which contains the result of the Epos export. This field is filterable allowing the user to list all the orders that failed for whatever reason. Orders can also be manually exported to the EPOS system with a new mass action 'Export To Epos'. Simply tick the orders to export and choose this option the Actions dropdown.

By default successfully exported orders cannot be exported again however there is a setting in the configuration to change this. This can be usefull for testing so that the same order can be processed repeatedly but it is recommended that it is set so that previously successful orders cannot be re-exported.

> When connecting to a SOAP wsdl URL it should be noted that most EPOS systems are behind firewalls and will require the IP of the server to be white listed before a successful connection.

For testing SOAP based requests and responses I reccommend using the SoapUI application before starting the PHP integration.

The Epos module contains two custom tables to track the order and stock processing, it also sends an email to email addresses set in the configuration.

There is an option to run a full reindex after the stock update, in the default Magento 2 a reindex is not required because updating the stock will trigger a partial reindex of the product updated however with other custom modules a reindex can be required. 





