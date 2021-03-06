===========================
InfluxDB, Grafana, Cronitor
===========================

.. image:: https://img.shields.io/circleci/project/github/Robpol86/influxdb/master.svg?style=flat-square&label=CircleCI
    :target: https://circleci.com/gh/Robpol86/influxdb
    :alt: Build Status

.. image:: docs/_static/grafana_filesrv.png
    :target: docs/_static/grafana_filesrv.png

.. summary-section-start

This is the `docker-compose <https://docs.docker.com/compose>`_ stack I use for my home network's monitoring. It
contains the following containers:

* `NGINX <https://www.nginx.com/>`_ SSL termination for InfluxDB and Grafana traffic outside of Docker containers.
* `InfluxDB <https://docs.influxdata.com/influxdb>`_ to store all of the metrics (the central piece of the setup).
* `Grafana <http://grafana.org>`_ to show pretty graphs and email me alerts.
* `Cronitor <https://cronitor.io>`_ for external monitoring of my Docker host and home network.
* `NMC`_ to include my UPS load, remaining runtime, and bedroom/living room temperatures in my Grafana graphs.

I also use the following services:

* `SparkPost <https://www.sparkpost.com/pricing>`_ free tier for sending alerts emails.

.. _NMC: http://www.apc.com/shop/us/en/products/UPS-Network-Management-Card-2-with-Environmental-Monitoring/P-AP9631

.. summary-section-end

Documentation
=============

`Visit the documentation <https://robpol86.github.io/influxdb>`_ for instructions on how to use this repository for your
own metrics/graphing setup at home. I've documented everything I need to get up and running on my server in case I ever
need to replace it from scratch.

Quick Start
===========

.. clone-section-start

Clone this repo locally (or fork it and clone that repo) to your Docker host (I put mine in ``/opt/influxdb``). Also
prepare your secrets by creating empty files with the right permissions:

.. code-block:: bash

    sudo mkdir /opt/influxdb; cd $_
    sudo git clone https://github.com/Robpol86/influxdb.git .
    sudo mkdir -m0750 .secrets
    for f in cronitor grafana.ini nmc; do
        sudo touch .secrets/$f; sudo chmod 0600 $_
    done

Next you'll want to glance over the various configuration files in this repo. They work for me but you may have a
different setup. Start with `docker-compose.yml <https://github.com/Robpol86/influxdb/blob/master/docker-compose.yml>`_
and look at other configuration files in this repository.

.. clone-section-end
.. up-section-start

Once everything in docker-compose.yml looks good go ahead and start the containers:

.. code-block:: bash

    cd /opt/influxdb; sudo docker-compose up -d
    sudo firewall-cmd --permanent --add-port=8086/tcp
    sudo firewall-cmd --permanent --add-port=3000/tcp
    sudo systemctl restart firewalld.service

Verify everything works by running ``sudo docker ps``. You should see "influxdb" and "grafana" in the NAMES column.

.. up-section-end
