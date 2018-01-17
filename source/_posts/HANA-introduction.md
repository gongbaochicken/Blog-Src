---
title: Introduction of SAP HANA
date: 2017-03-09 22:50:20
tags:
  - SAP HANA
categories:
  - Database
---
I joined SAP Labs about 1 year ago, and one of the reasons is that I think SAP HANA is a powerful data storage platform, which requires strong challenges of back end computer engineering. This post I will briefly describe what kind of database HANA is.
<!--more-->

## Introduction

According to the official description, SAP HANA is an in-memory, column-oriented, relational database management system developed and marketed by SAP SE. Its primary function as database server is to store and retrieve data as requested by the applications. In addition, it performs advanced analytics (predictive analytics, spatial data processing, text analytics, text search, streaming analytics, graph data processing) and includes ETL capabilities as well as an application server. It is very powerful when it works with other SAP enterprise software.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489120995/Screen_Shot_2017-03-09_at_10.16.43_PM_ra8cjq.bmp)

## Features:

### 1. High Performance

Since SAP HANA uses main memory as data store, you can guess it is fast.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489121339/1345039203_7744_hjakht.jpg)

According to the above comparison, memory is 10X~100X faster than SSD, flash and disk, so storing data in memory can amazingly accelerate the data retrieve and processing efficiency. Traditional database has the danger of losing data when data is saved in main memory(Power Failure, Reboot), however SAP HANA has a asynchronous savepoint as data persistence to protect the data. Therefore, HANA can safely and easily load the data to CPU cache from the memory, and this can break the bottleneck of traditional disk IO.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489121346/1345049766_6976_oznwqq.jpg)

### 2. Hardware

For high speed processing and big throughput, it is better to choose high-performance multiple-core 64bit linux server. Thanks to the innovations of hardware industry, HANA is able to leverage the capabilities of high-performance hardware to process parallel computing, data partition, and data cleanse.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489121040/Screen_Shot_2017-03-09_at_10.31.48_PM_xmq52h.bmp)

I use 24-core SUSE linux in SAP labs, and our common performance testing machines are 40-core linux box. But there are plenty of choices in appliances, linux systems, and storage providers.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489120852/Screen_Shot_2017-03-09_at_10.18.05_PM_pujudi.bmp)

### 3. Data Compression
SAP HANA can support both row-based and column-based storage. One of the innovations of SAP HANA is that thanks to column-based storage, the data can be compressed to save storage space and reduce data transmission.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489121055/Screen_Shot_2017-03-09_at_10.35.06_PM_setnxt.bmp)

This is an interesting topic, and I may have interest to write another post to describe this part soon later.

### 4. Minimizing data transfer

In traditional database, software or an application will read the data from database, do business logic computing in application layer, and finally save the result back to database.

HANA, however, can push computing logic to the database, and there is no need to transfer data back and forth. It cuts off a lot of data transfer time and network resource, and reduces delay.

### 5. Failure Tolerance

One of the failure protection is that SAP HANA has a asynchronous savepoint as data persistence to protect the data from Power Failure or unexpected reboot.

Also, it supports multiple tenants and backup servers, which will server as addition replica of master server.

![](http://res.cloudinary.com/dxdsd8err/image/upload/v1489121019/Screen_Shot_2017-03-09_at_10.28.45_PM_ydjdmh.bmp)

## Who uses SAP HANA?
More than 7000 business customers are using SAP HANA， SAP HANA cloud, SAP S/4HANA, and usually they are big companies, including P&G, Shell, T-Mobile, etc.

![](https://image.slidesharecdn.com/saphanacloudportal-november2013-131121063933-phpapp01/95/sap-hana-cloud-portal-overview-presentation-6-638.jpg?cb=1385016090)

Not the same as Amazon or Google serving personal customers, SAP serves enterprises and it is only shining on the back of stage. But you have to admit that you may indirectly use our product, and you just don't notice it before. LoL.

## Future

Currently, more and more enterprise customers are cooperating with SAP and using HANA. And SAP and Google just announced strategic partnership of infrastructure-as-a-service to lift enterprise cloud applications to the next level. As a developer of SAP HANA, I feel proud of this promising product.

## Reference
1. https://www.slideshare.net/SAPTechnology/sap-hana-platform-overview
