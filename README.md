# EFD-TELEMETRY
SASQUATCH: How To Access The EFD ( Engineering and Facilities Database ), Vera Rubin Observatory - LSST ( Large Synoptic Survey Telescope).

Sasquatch is the Rubin Observatory’s service for metrics and telemetry data.
Built on Kafka and InfluxDB, it offers a comprehensive solution for collecting, storing, and querying time-series data.
Sasquatch is currently deployed at the Summit, USDF and test stands through Phalanx (a GitOps repository for Rubin Observatory’s Kubernetes environments, notably including Rubin Science Platform deployments).
Rubin Observatory generates a large amount of engineering data, which
is stored in the Engineering and Facilities Database (EFD). 

In this paper there is a brief guide on how to access the EFD from the different environments available by focusing on the USDF Chronograf which is accessible without Chile VPN, and a huge part is dedicated to the EdfClient Python client on how we can get the list of all available Telemetry Topics in SAL scripts, all the related fields in each topic and the list of dictionaries describing these fields. Several example are given on specific mechanical parts of the telescope.

Sasquatch is the Rubin Observatory’s service for metrics and telemetry data, it provides a detailed User Guide for the Observatory Telemetry (EFD - Engineering and Facilities Database), Analysis Tools metrics and Data exploration and visualization with Chronograf. Sasquatch provides also different \href{https://sasquatch.lsst.io/environments.html}{Environments}, a \href{https://sasquatch.lsst.io/developer-guide/index.html}{Developer Guide} and technical docs and software in \href{https://www.lsst.io/}{Rubin Docs}.
In Sec. \ref{env} we make a brief overview on the available environments, in Secs. \ref{summit}-\ref{bts} we give a brief explanation of the Summit and BTS environments, in Sec. \ref{usdf} we explain how to access and how to use the \href{https://usdf-rsp.slac.stanford.edu/chronograf}{USDF (US Data Facility)}, in Sec. \ref{efd} we show how to access the \href{https://usdf-rsp.slac.stanford.edu/}{USDF Rubin Science Platform} with some example notebooks and finally in Sec. \ref{link} we find some useful links to the LSST (Large Synoptic Survey Telescope) instrumentation.
