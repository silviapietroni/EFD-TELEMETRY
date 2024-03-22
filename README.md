# EFD-TELEMETRY
SASQUATCH - How To Access The EFD ( Engineering and Facilities Database ) - Vera Rubin Observatory - LSST ( Large Synoptic Survey Telescope)
\documentclass{article}
\usepackage{graphicx} % Required for inserting images
\usepackage{url}
\usepackage{hyperref}
\usepackage{float}

% Preambolo
\usepackage{listings} % Per includere il codice sorgente
\usepackage{xcolor} % Per definire i colori personalizzati

% Definizione dello stile per il codice Python
\lstdefinestyle{mystyle}{
    language=Python,
    basicstyle=\footnotesize\ttfamily,
    keywordstyle=\color{green},
    commentstyle=\color{green},
    stringstyle=\color{purple},
    numbers=left,
    numberstyle=\tiny\color{violet},
    stepnumber=1,
    numbersep=5pt,
    backgroundcolor=\color{white},
    frame=single,
    tabsize=50,
    captionpos=b,
    breaklines=true,
    breakatwhitespace=false,
    showspaces=false,
    showstringspaces=false,
    showtabs=true,
    morekeywords={True, False}
}

\title{Sasquatch}
\author{Silvia Pietroni\\INAF OACN\\ Vera Rubin - LSST}
\date{February 2024}

\begin{document}

\maketitle

\begin{abstract}Sasquatch is the Rubin Observatory’s service for metrics and telemetry data.
Built on Kafka and InfluxDB, it offers a comprehensive solution for collecting, storing, and querying time-series data.
Sasquatch is currently deployed at the Summit, USDF and test stands through Phalanx (a GitOps repository for Rubin Observatory’s Kubernetes environments, notably including Rubin Science Platform deployments).
Rubin Observatory generates a large amount of engineering data, which
is stored in the Engineering and Facilities Database (EFD). 

In this paper there is a brief guide on how to access the EFD from the different environments available by focusing on the USDF Chronograf which is accessible without Chile VPN. A huge part is dedicated to the EdfClient Python client on how we can get the list of all available Telemetry Topics in SAL scripts, all the related fields in each topic and the list of dictionaries describing these fields. Several example are given on specific mechanical part of the telescope.
\end{abstract}

\section{Introduction}


Sasquatch is the Rubin Observatory’s service for metrics and telemetry data, it provides a detailed \href{https://sasquatch.lsst.io/user-guide/index.html}{User Guide} for the Observatory Telemetry (EFD - Engineering and Facilities Database), Analysis Tools metrics and Data exploration and visualization with Chronograf. Sasquatch provides also different \href{https://sasquatch.lsst.io/environments.html}{Environments}, a \href{https://sasquatch.lsst.io/developer-guide/index.html}{Developer Guide} and technical docs and software in \href{https://www.lsst.io/}{Rubin Docs}.
In Sec. \ref{env} we make a brief overview on the available environments, in Secs. \ref{summit}-\ref{bts} we give a brief explanation of the Summit and BTS environments, in Sec. \ref{usdf} we explain how to access and how to use the \href{https://usdf-rsp.slac.stanford.edu/chronograf}{USDF (US Data Facility)}, in Sec. \ref{efd} we show how to access the \href{https://usdf-rsp.slac.stanford.edu/}{USDF Rubin Science Platform} with some example notebooks.


\section{Environments}\label{env}

In each Sasquatch \href{https://sasquatch.lsst.io/environments.html}{Environment} we have a Chronograf according to the following Table \ref{saenv}:

\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{env.png}
    \caption{Sasquatch Environments.}
    \label{saenv}
\end{figure}


For our purpose we need the \href{https://summit-lsp.lsst.codes/chronograf}{Summit}, the \href{https://base-lsp.lsst.codes/chronograf}{Base Test Stand (BTS)} and the \href{https://usdf-rsp.slac.stanford.edu/chronograf}{USDF (US Data Facility)} Environments with their respective Chronographs. The Summit and the BTS Chronographs requires the Chile VPN and other authorizations, while it is possible to access the USDF Chronographs by logging with your own institutional LSST account (S3DF or SLAC). 

For any technical issue with these environments it is possible to drop a line at \#com-square-support on LSSTC Channel on Slack.

% and the \href{https://usdf-rsp-dev.slac.stanford.edu/chronograf}{USDF dev}

\section{Summit}\label{summit}

\href{https://summit-lsp.lsst.codes/chronograf}{Summit} is the main production operational environment. Systems running here will be directly controlling hardware or communicating with components that control actual hardware.

The EFD is used at the Summit for real-time monitoring of the observatory systems using Chronograf dashboards and alerts.
This instance collects engineering data from the Summit and is the primary source of EFD data.

Intended audience: Observers and Commissioning team at the Summit.

%il summit dopo un pò muore

\section{Base Test Stand - BTS}\label{bts}

\href{https://base-lsp.lsst.codes/chronograf}{The Base Test Stand (BTS)} is a simulation operational environment running at the base facility in La Serena. Some systems will run using hardware simulators.
Hardware simulators are higher fidelity than those of pure software, for that, BTS will be used mostly for higher fidelity testing, development and debugging. It will also be particularly useful for testing lower-level hardware interfaces.

Intended audience: Telescope \& Site team.


%roba di cloud, per fare sviluppo di script per fare dei test ( testare gli script prima di farli sull'hardware vero) 

%\subsection{USDF- dev}Intended audience: Project staff.


\section{USDF -  US Data Facility }\label{usdf}
The EFD data is also replicated to the US Data Facility (USDF) for long-term storage and analysis.
%This instance has EFD data replicated in real-time from the Summit.

Intended audience: Project staff.

%database storico, per fare monitoraggi

Once into the \href{https://usdf-rsp.slac.stanford.edu/chronograf}{USDF Chronograf}, we have this screen with a lateral vertical bar as shown in Fig.\ref{status}:

\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{usdfstatus.png}
    \caption{USDF Chronograf Status.}
    \label{status}
\end{figure}

then we can move down to EXPLORE on the left bar as in Fig.\ref{exp}:



\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{usdfexplore.png}
    \caption{USDF Chronograf Explore.}
    \label{exp}
\end{figure}



From the Dashboard, see Fig.\ref{dash},


\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{usdfdashboard.png}
    \caption{USDF Chronograf Dashboard.}
    \label{dash}
\end{figure}


 we can select a component from LSST (please see \href{https://rubinobservatory.org/explore/technology}{Rubin Technology}), for example the MTMount (\href{https://rubinobservatory.org/explore/technology/mount}{the Telescope Mount}) where we can see all the variables such as the azimuth or the elevation (see Fig.\ref{mtmount}) at different samples and different epochs (minutes, hours, or, with the Data Picker, see Fig.\ref{datapick}, we can choose a specific date, at the top right of the Dashboard).


\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{mtmount.png}
    \caption{USDF Chronograf: MT Mount Variables.}
    \label{mtmount}
\end{figure}


\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{datapick.png}
    \caption{USDF Chronograf: Data Picker.}
    \label{datapick}
\end{figure}


It is also possible, on each single variable, to download the respective CSV file, as shown in the example in Fig.\ref{csv}, and in Fig.\ref{csvfile} we can see the output of the CSV file.

\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{csv.png}
    \caption{USDF Chronograf: how to download a CSV file from a single variable.}
    \label{csv}
\end{figure}

\begin{figure}[H]
    \centering
    \includegraphics[height=9.5 cm]{csvfile.png}
    \caption{Example of a CSV file for a single variable (the azimuth in this example).}
    \label{csvfile}
\end{figure}


In Fig.\ref{mtm2} there is another example for MTM2, one of the \href{https://rubinobservatory.org/explore/technology/mirrors}{Mirrors}:


\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{mtm2.png}
    \caption{MTM2 variables from the Dashboard.}
    \label{mtm2}
\end{figure}


\section{The RSP and the EFD Python Client}\label{efd}

It is possible also to access the Chronograf through a Python script.

It is necessary to go on the \href{https://usdf-rsp.slac.stanford.edu/}{USDF RSP (Rubin Science Platform)} (see Fig.\ref{rsp})

\begin{figure}[H]
    \centering
    \includegraphics[width=11 cm]{usdfscienceplat.png}
    \caption{USDF RSP (Rubin Science Platform)}
    \label{rsp}
\end{figure}



and to click on Notebooks where some tutorial-notebooks are available. Then it is possible to read the User Guide at the \href{https://sasquatch.lsst.io/user-guide/efdclient.html}{The EFD Python client} where there is the explanation on how to instantiate the EFD client with a Python script and where it is possible to create a notebook on the \href{https://usdf-rsp.slac.stanford.edu/}{USDF Rubin Science Platform} in order to get the EFD data.





There are Python functions and classes in the \href{https://efd-client.lsst.io/api.html}{Python API reference documentation}.
Among the classes to handle connections and basic queries asynchronously we can refer to  \href{https://efd-client.lsst.io/api/lsst_efd_client.EfdClient.html#lsst_efd_client.EfdClient}{EfdClient}.

\subsection{Example Notebook}

We give a detailed explanation of an example notebook able to access the EFD which is available on \href{https://github.com/silviapietroni}{GitHub - Silvia Pietroni}.

\subsubsection{Available Topics and Available Variables in SAL Scripts}

First we initiate our code with:

\begin{lstlisting}[style=mystyle,  label=python-example]
from lsst_efd_client import EfdClient 
efd_client = EfdClient("usdf_efd")
\end{lstlisting}


then, in order to get all the available topics, we can use \href{https://ts-salobj.lsst.io/sal_scripts.html#script-packages}{SAL scripts} which are programs that perform coordinated telescope and instrument control operations; in our case we have a list of \href{https://sqr-085.lsst.io/#recommendations}{Telemetry Topics} or we are able to get a list through this command:

\begin{lstlisting}[style=mystyle,label=python-example]
topics = await efd_client.get_topics()
\end{lstlisting}

which gives more than 2320 topics, in Fig.\ref{topics} a fraction of the list of topics available for the MTM2 and for the MTMount:

\begin{figure}[H]
    \centering
    \includegraphics[width= 10 cm]{salcom.png}
    \caption{Some of the SAL commands available for the MTM2 and for the MTMount.}
    \label{topics}
\end{figure}


We can get all the available variables (fields) in a single topic with the \textbf{get\_fields(topic\_name)} method:

\begin{lstlisting}[style=mystyle,label=python-example]
await efd_client.get_fields("lsst.sal.MTM2.position")   
\end{lstlisting}

and we can get, from a given topic, a list of dictionaries describing the fields with the \textbf{get\_schema(topic)} method:

\begin{lstlisting}[style=mystyle,label=python-example]
await efd_client.get_schema("lsst.sal.MTM2.position")   
\end{lstlisting}

in Fig.\ref{pos} an example of a list of dictionaries for the MTM2 position:



\begin{figure}[H]
    \centering
    \includegraphics[width= 10 cm]{schema.png}
    \caption{An example of a list of dictionaries for the MTM2 position.}
    \label{pos}
\end{figure}


\subsubsection{Time Series: Samples in a Specific Time Interval}

If we define a specific time interval, for example:

\begin{lstlisting}[style=mystyle,label=python-example]
t_start = Time("2024-02-23T19:20:00", scale="utc", format="isot")
t_end = Time("2024-02-23T19:25:00", scale="utc", format="isot")

\end{lstlisting}


we are able to get, from a topic, all the variable values in that specific time interval with the \textbf{select\_time\_series(topic\_name, fields=[], start, end)} method, her and example for the MTM2 position:

\begin{lstlisting}[style=mystyle,label=python-example]
await efd_client.select_time_series("lsst.sal.MTM2.position",
fields=["x","xRot","y","yRot","z","zRot","private_efdStamp",
"private_host","private_identity", "private_kafkaStamp",
"private_origin","private_rcvStamp","private_revCode",
"private_seqNum","private_sndStamp"],
start=t_start, end=t_end)   
\end{lstlisting}

and we get the table in Fig.\ref{select}:

\begin{figure}[H]
    \centering
    \includegraphics[width= 10 cm]{select.png}
    \caption{Table for a specific time interval and for specific variables.}
    \label{select}
\end{figure}

Other examples on more SAL Scripts and variables of different instruments are listed in the notebook (MTM2, MTM1M3, AT Camera, ATMCS (Auxiliary Telescope Mount Control System)) but in order do get data it is necessary to know a specific time interval in which measurements are available.

\subsection{Queries with InfluxQL}


To perform queries directly using \href{https://sasquatch.lsst.io/user-guide/efdclient.html}{InfluxQL} (an SQL-like query language for interacting with data in InfluxDB) we can make use of the \textbf{influx\_client.query()} method in conjunction with the EFD client.

For a comprehensive understanding of the InfluxQL query syntax, we recommend referring to the \href{https://docs.influxdata.com/influxdb/v1/query_language/explore-data/}{InfluxQL documentation}.

With the \textbf{build\_time\_range\_query((topic\_name, fields, start, end)} method we can build a query based on a time range as we can see in the following example for the net moment of force in the x direction:

\begin{lstlisting}[style=mystyle,label=python-example]
query=efd_client.build_time_range_query("lsst.sal.MTM2.netMomentsTotal", fields=["mx","my","mz"], start=t_start, end=t_end)
\end{lstlisting}

which gives:

\begin{verbatim}
    'SELECT mx, my, mz FROM "efd"."autogen"."lsst.sal.MTM2.netMomentsTotal"
    WHERE time >= \'2024-02-23T19:20:00.000Z\' AND time <= \'2024-02-23T19:25:00.000Z\''
\end{verbatim}




%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
-------------------------------

In Fig.\ref{pyscript} an example of script on how to get some CSV files by selecting a specific time interval and the parameters needed, and then the CSV output in Fig.\ref{pycsv}.




%\begin{figure}[H]
%    \centering
%    \includegraphics[height=8  cm]{pyscript.png}
%    \caption{A Python script to access EFD.}
%    \label{pyscript}
%\end{figure}

%\begin{figure}[H]
 %   \centering
 %   \includegraphics[height=8 cm]{pycsv.png}
  %  \caption{A CSV file from a Python script.}
 %   \label{pycsv}
%\end{figure}



Other examples on querying the EFD with InfluxQL (an SQL-like query language for interacting with data in InfluxDB) can be found on the Angelo Fausti's (the Software Engineer working on the EFD and Sasquatch) \href{https://github.com/lsst-sqre/sasquatch/blob/main/docs/user-guide/notebooks/UsingInfluxQL.ipynb}{Github}.




\section{LSST Instrumentation}

\begin{itemize}
    \item \href{https://www.lsst.org/about/camera}{Camera} 
    \item \href{https://rubinobservatory.org/explore/technology/camera}{Camera } (second link)
  
\item \href{https://rubinobservatory.org/explore/technology/mirrors}{Mirrors}
    \item  \href{https://rubinobservatory.org/explore/technology/mount}{Telescope Mount}
    
%\item \href{https://ts-atmcs.lsst.io/}{ATMCS (Auxiliary Telescope Mount Control System)}

    \item \href{https://ts-observatory-control.lsst.io/user-guide/auxtel/atcs-user-guide.html}{ATCS (Auxiliary Telescope Control System )} which comprehends ATPtg, ATMCS, ATAOS, ATDomeTrajectory, ATDome, ATPneumatics, ATHexapod)


    \item \href{https://rubinobservatory.org/explore/technology/summit-facility}{Observatory Facility}
\end{itemize}







\end{document}

