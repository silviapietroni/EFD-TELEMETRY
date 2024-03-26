# EFD-TELEMETRY
SASQUATCH: How To Access The EFD ( Engineering and Facilities Database ), Vera Rubin Observatory - LSST ( Large Synoptic Survey Telescope).

Sasquatch is the Rubin Observatory’s service for metrics and telemetry data.
Built on Kafka and InfluxDB, it offers a comprehensive solution for collecting, storing, and querying time-series data.
Sasquatch is currently deployed at the Summit, USDF and test stands through Phalanx (a GitOps repository for Rubin Observatory’s Kubernetes environments, notably including Rubin Science Platform deployments).
Rubin Observatory generates a large amount of engineering data, which
is stored in the Engineering and Facilities Database (EFD). 

In this paper there is a brief guide on how to access the EFD from the different environments available by focusing on the USDF Chronograf which is accessible without Chile VPN, and a huge part is dedicated to the EdfClient Python client on how we can get the list of all available Telemetry Topics in SAL scripts, all the related fields in each topic and the list of dictionaries describing these fields. Several example are given on specific mechanical parts of the telescope.

Sasquatch is the Rubin Observatory’s service for metrics and telemetry data, it provides a detailed User Guide (https://sasquatch.lsst.io/user-guide/index.html) for the Observatory Telemetry (EFD - Engineering and Facilities Database), Analysis Tools metrics and Data exploration and visualization with Chronograf. Sasquatch provides also different Environments (https://sasquatch.lsst.io/environments.html), a Developer Guide (https://sasquatch.lsst.io/developer-guide/index.html) and technical docs and software in Rubin Docs (https://www.lsst.io/).
In Sec. 2 we make a brief overview on the available environments, in Secs. 3-4 we give a brief explanation of the Summit and BTS environments, in Sec. 5 we explain how to access and how to use the USDF (US Data Facility) (https://usdf-rsp.slac.stanford.edu/chronograf), in Sec. 6 we show how to access the USDF Rubin Science Platform (https://usdf-rsp.slac.stanford.edu/) with some example notebooks and finally in Sec. 7 we find some useful links to the LSST (Large Synoptic Survey Telescope) instrumentation.
