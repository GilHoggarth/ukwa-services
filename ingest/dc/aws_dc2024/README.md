Domain Crawl 2024
=================

####
* IMPORTANT: Scripts starting with 'z' are for development purposes and must not be used on a production service.
####

This directory contains the settings and deployment scripts for the DC2024 operation on AWS.

Before running the deploy script:
- Copy a past .env file and edit the values for this server.
- Ensure that the new .env file name includes this server's name so that it can be easily identified.
- Ensure that the STORAGE_PATH directory in the .env file exists and is owned by this user (not root). If it doesn't, the server probably isn't ready for the domain crawl deployment.


Step 1: Create required directories
-----------------------------------

By running the `dc0` script with the new .env file, the required directories will be created. Note that the primary directory - STORAGE_PATH - has to exist for this script to complete. This attempts to ensure that if extra volumes need to be created and mounted beforehand, this extra setup step is done before running the create directories script. For example,
* `./dc0-initialise.sh aws_dc2024_crawler08-prod.env`

Kafka
-----

The first service required is the kafka queue manager and UI. To deploy:
* `./dc1-deploy_aws_dc_kafka.sh aws_dc2024_crawler08-prod.env`

After the kafka service is deployed, the kafka queues (known as topics in Kafka) need to be created, by:
* `./dc2-create_kafka_topics.sh aws_dc2024_crawler08-prod.env`

Viewing the Kafka UI now should show **dc_cluster** under the Dasboard, and within this dc_cluster, 3 topics should exist for the domain crawl: dc.tocrawl, dc.inscope, and dc.crawled. There should also be a validated broker showing the EC2 internal IP.
