---
sidebar_label: 'OpenVINO™ Telemetry'
format: md
---

  - orphan

# OpenVINO™ Telemetry

To facilitate debugging and further development, OpenVINO™ collects anonymous telemetry data. Anonymous telemetry data is collected by default,
but you can stop data collection anytime by running the command `opt_in_out --opt_out`.
It does not extend to any other Intel software, hardware, website usage, or other products.

Google Analytics is used for telemetry purposes. Refer to
[Google Analytics support](https://support.google.com/analytics/answer/6004245#zippy=%2Cour-privacy-policy%2Cgoogle-analytics-cookies-and-identifiers%2Cdata-collected-by-google-analytics%2Cwhat-is-the-data-used-for%2Cdata-access) to understand how the data is collected and processed.

## Enable or disable Telemetry reporting

### Changing consent decision

You can change your data collection decision with the following command lines:

`opt_in_out --opt_in` - enable telemetry

`opt_in_out --opt_out` - disable telemetry

## Telemetry Data Collection Details

Telemetry Data Collected

  - Failure reports
  - Error reports
  - Usage data

Tools Collecting Data

  - Model conversion API
  - Model Downloader
  - Accuracy Checker
  - Post-Training Optimization Toolkit
  - Neural Network Compression Framework
  - Model Converter
  - Model Quantizer

Telemetry Data Retention

Telemetry data is retained in Google Analytics for a maximum of 14 months.
Any raw data that has reached the 14-month threshold is deleted from Google Analytics on a monthly basis.
