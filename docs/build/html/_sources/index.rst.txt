.. dreiAttest - White Paper documentation master file, created by
   sphinx-quickstart on Fri Oct  1 09:11:26 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to the dreiAttest White Paper!
====================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

You've found our white paper going into the technical details of how dreiAttest works behind the scenes. For information on how to use dreiAttest in your project check out the GitHub page of the corresponding package:

- iOS_
- `Android / KMP`_
- Server_

.. _iOS: https://github.com/dreipol/dreiAttest-ios/
.. _Android / KMP: https://github.com/dreipol/dreiAttest-android/
.. _Server: https://github.com/dreipol/django-dreiattest/

Overview
========
dreiAttest is a framework built on top of Apple’s DeviceCheck_ and Google’s SafetyNet_. It protects services by restricting access to the corresponding App and genuine iOS / Android devices, e.g. preventing access via scripts. Furthermore, dreiAttest can be extended through plug-ins to enforce different policies such as evaluating Apple’s `risk metric`_ or by using the `per-device data`_.

.. _risk metric: https://developer.apple.com/documentation/devicecheck/assessing_fraud_risk
.. _per-device data: https://developer.apple.com/documentation/devicecheck/accessing_and_modifying_per-device_data
.. _DeviceCheck: https://developer.apple.com/documentation/devicecheck
.. _SafetyNet: https://developer.android.com/training/safetynet

Limits
------
DeviceCheck / SafetyNet cannot determine the integrity of the operating system. Under some circumstances it may also be possible to manipulate the local data stored by an app without it being possible to detect such tampering via DeviceCheck / SafetyNet.
Furthermore `DeviceCheck cannot be used in App Extensions`_.

.. _DeviceCheck cannot be used in App Extensions: https://developer.apple.com/documentation/devicecheck/establishing_your_app_s_integrity

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
